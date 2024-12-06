#include <iostream>
#include <stdexcept> // For exceptions
#include <sstream>   // For stringstream to format the print function

// Vector struct definition
struct Vector {
    int* arr;       // Dynamic array to store the integers
    int size;       // Number of valid elements in the vector
    int capacity;   // Total capacity of the vector

    // Constructor to initialize the vector with size 0 and capacity 2
    Vector() : size(0), capacity(2) {
        arr = new int[capacity]; // Initialize dynamic array with capacity 2
    }

    // Destructor to free allocated memory
    ~Vector() {
        delete[] arr;
    }
};

// Function to add an item at the end of the vector
void add(Vector& vector, int item) {
    if (vector.size == vector.capacity) {
        // Double the capacity when the vector is full
        vector.capacity *= 2;
        int* newArr = new int[vector.capacity];

        // Copy existing elements to the new array
        for (int i = 0; i < vector.size; ++i) {
            newArr[i] = vector.arr[i];
        }

        // Delete the old array and point to the new array
        delete[] vector.arr;
        vector.arr = newArr;
    }

    // Add the new item and increase the size
    vector.arr[vector.size] = item;
    vector.size++;
}

// Function to remove and return the element at the specified index
void remove(Vector& vector, int index) {
    if (index < 0 || index >= vector.size) {
        throw std::out_of_range("Error: Index out of range for remove.");
    }

    int removedItem = vector.arr[index];  // Get the item to be removed

    // Shift elements to the left after the removed item
    for (int i = index; i < vector.size - 1; ++i) {
        vector.arr[i] = vector.arr[i + 1];
    }

    // Decrease the size
    vector.size--;
}

// Function to update the element at the specified index with a new value
void set(Vector& vector, int index, int value) {
    if (index < 0 || index >= vector.size) {
        throw std::out_of_range("Error: Index out of range for set.");
    }

    // Update the value at the specified index
    vector.arr[index] = value;
}

// Function to print the vector's information
void print(const Vector& vector) {
    std::cout << "Size: " << vector.size << ", Capacity: " << vector.capacity << ", Elements: [";

    for (int i = 0; i < vector.size; ++i) {
        std::cout << vector.arr[i];
        if (i < vector.size - 1) std::cout << ", ";
    }

    std::cout << "]" << std::endl;
}

// Main test program
int main() {
    try {
        Vector v;

        // 1) Create a vector and print its initial state
        std::cout << "Initial vector: ";
        print(v);

        // 2) Add 4 items to the vector and print the updated vector
        for (int i = 1; i <= 4; ++i) {
            add(v, i);
        }
        std::cout << "After adding 4 items: ";
        print(v);

        // 3) Add 1 more item to the vector and print
        add(v, 5);
        std::cout << "After adding 1 more item: ";
        print(v);

        // 4) Call set() with a valid index and value
        set(v, 2, 99); // Set element at index 2 to 99
        std::cout << "After setting index 2 to 99: ";
        print(v);

        // 5) Print the updated vector
        std::cout << "Final vector: ";
        print(v);

        // 6) Call remove() with a valid index
        remove(v, 1); // Remove element at index 1
        std::cout << "After removing element at index 1: ";
        print(v);

        // 7) Print the vector after removal
        std::cout << "Final vector after removal: ";
        print(v);

        // 8) Call set() with an invalid index and handle the exception
        try {
            set(v, 10, 100); // Invalid index
        } catch (const std::exception& e) {
            std::cout << e.what() << std::endl;
        }

        // 9) Call remove() with an invalid index and handle the exception
        try {
            remove(v, 10); // Invalid index
        } catch (const std::exception& e) {
            std::cout << e.what() << std::endl;
        }

        // 10) Print "Done!" at the end
        std::cout << "Done!" << std::endl;

    } catch (const std::exception& e) {
        std::cout << "Exception: " << e.what() << std::endl;
    }

    return 0;
}
