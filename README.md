# VAutocomplete Infinite Scroll Component

This repository provides a Vue 3 component using Vuetify 3 that enhances the VAutocomplete functionality to allow for multiple user selection. It enables users to search for and select multiple items from a dynamically loaded list, with selected items displayed as VChips for easy management.

## Features

- **Multi-Select Functionality**: Users can select multiple items from the autocomplete list.
- **Dynamic Search**: Real-time search updates the list of available options based on user input.
- **Infinite Scrolling**: Additional users are loaded as the user scrolls, ensuring a seamless experience.
- **Loading Indicators**: Visual feedback informs users when more data is being loaded or when all data has been loaded.
- **Clearable Selection**: Users can easily clear their selected items.

## Installation

To integrate this component into your project, follow these steps:

1. **Clone the repository**:
   ```sh
   git clone https://github.com/YoussefAMansour/VAutocomplete-Infinite-Scroll-Multi-Select
2. **Navigate to the project directory**:
    ```sh
   cd v-autocomplete-multi-select

3.  **Install dependencies**:
    ```sh
    npm install
    
4.  **Run the project**:
    ```sh
    npm run serve

## Usage

Here's an example of how to use the VAutocomplete Multi-Select component in your Vue 3 application:
1. ```shell
    <template>
        <div>
        <VAutocompleteMultiSelect  />
      </div>
    </template>

    <script setup>
        import VAutocompleteMultiSelect  from '@/components/VAutocompleteMultiSelect.vue';
    </scrpit>

## Configuration

The component can be customized through several props and slots:

Props:
* items: Array of items to be displayed in the autocomplete.
* item-text: Property name to display in the list.
* item-value: Property name to use as the value.
* label: Label for the autocomplete input.
* search-input.sync: Two-way binding for the search input.
* multiple: Boolean to enable multi-select functionality.
* clearable: Boolean to enable clearing the selection.
* autocomplete: Attribute to disable browser autocomplete.


Slots:
* append-item: Custom content to append at the end of the list, typically used for loading indicators.


## Contributing

Contributions are welcome! If you have any ideas, suggestions, or improvements, feel free to open an issue or submit a pull request.

## License

This project is licensed under the MIT License. See the LICENSE file for more details.
