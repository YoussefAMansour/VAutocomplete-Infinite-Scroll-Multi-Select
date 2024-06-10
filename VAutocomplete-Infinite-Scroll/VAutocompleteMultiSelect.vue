<template>
  <div>
    <!-- VAutocomplete component for searching users -->
    <VAutocomplete
        v-model="selected"
        :items="users.map(user => user.name)"
        item-text="name"
        item-value="id"
        label="Search users"
        :search-input.sync="search"
        return-object
        multiple
        clearable
        autocomplete="off"
        ref="autocompleteRef"
        @input="infiniteSearch"
        @blur="handleBlur"
    >
      <!-- Loading indicator or message when reaching end of data -->
      <template v-slot:append-item>
        <div ref="bottomOfList" class="pa-4 teal--text">
          <template v-if="users.length > 0 && !allDataLoaded">
            Loading more users...
          </template>
          <template v-else-if="!search && allDataLoaded && dataEverLoaded">
            No More Data.
          </template>
        </div>
      </template>
    </VAutocomplete>
    <br>
    <!-- Display selected items -->
    <VCol cols="12">
      <VRow flex-wrap v-if="selectedItemsWithIds.length > 0">
        <VCol cols="12">
          <h3>Selected Items:</h3>
          <br>
          <VRow>
            <VCol v-for="item in selectedItemsWithIds" :key="item.id" cols="auto">
              <VChip closable @click:close="removeItem(item)" variant="outlined">{{ item.name }}</VChip>
            </VCol>
          </VRow>
        </VCol>
      </VRow>
    </VCol>
  </div>
</template>

<script setup>
import {ref, watch, computed} from 'vue';
import {useCookie} from "@core/composable/useCookie";

// Variables
const selected = ref([]);
const users = ref([]);
const perPage = 10;
let page = 1;
let search = '';
const allDataLoaded = ref(false);
const dataEverLoaded = ref(false);
const accessToken = useCookie('accessToken').value;
const bottomOfList = ref(null);

// Fetch users based on search query
const getUsers = async () => {
  // Check if search query is empty or all data is already loaded
  if (search.trim() === '' || allDataLoaded.value) {
    console.log('No search query or all data loaded, not loading more data');
    return;
  }

  const apiUrl = `${Your_API}supplier_name_find?per_page=${perPage}&page=${page}&term=${search}`;
  const response = await fetch(apiUrl, {
    headers: {
      'Authorization': `Bearer ${accessToken}`
    }
  });
  if (!response.ok) {
    console.log('Network response was not ok');
    return;
  }
  const responseData = await response.json();
  const suppliersData = responseData.data;
  if (suppliersData.length < perPage) {
    allDataLoaded.value = true;
  }
  if (suppliersData.length > 0) {
    dataEverLoaded.value = true;
  }
  users.value = page === 1 ? suppliersData : [...users.value, ...suppliersData];
  page++;
};

// Function to handle infinite scrolling for search results
const infiniteSearch = (event) => {
  // Update search query and reset page and flags for new search
  search = event.target.value;
  page = 1;
  allDataLoaded.value = false;
  dataEverLoaded.value = false;
  users.value = [];
  getUsers();
};

// Initialize Intersection Observer for infinite scrolling
const initIntersectionObserver = () => {
  const options = {
    root: null,
    rootMargin: '0px',
    threshold: 1.0,
  };
  const callback = (entries) => {
    if (entries[0].isIntersecting && !allDataLoaded.value) {
      getUsers();
    }
  };
  // Create Intersection Observer instance
  const observer = new IntersectionObserver(callback, options);
  // Observe the bottomOfList element
  observer.observe(bottomOfList.value);
};

// Function to remove item from selected list
const removeItem = (itemToRemove) => {
  // Filter out the item to be removed from selected list
  selected.value = selected.value.filter(item => item !== itemToRemove.name);
};

// Watch for changes in bottomOfList and initiate Intersection Observer
watch(bottomOfList, (newValue) => {
  if (newValue) {
    // Initialize Intersection Observer
    initIntersectionObserver();
  }
});

// Watch for changes in search query and fetch users accordingly
watch(() => search, () => {
  // Reset page count and flags when search query changes
  page = 1;
  allDataLoaded.value = false;
  dataEverLoaded.value = false;
  users.value = [];
  // Fetch users based on new search query
  getUsers();
});

// Computed property to get selected items with their IDs
const selectedItemsWithIds = computed(() => {
  return selected.value.map(item => {
    // Find the corresponding user object from the users list
    const user = users.value.find(user => user.name === item);
    return {id: user ? user.id : null, name: item};
  });
});

// Clear users list if input is empty when the VAutocomplete loses focus
const handleBlur = () => {
  if (selected.value.length === 0) {
    users.value = []
  }
}
</script>
