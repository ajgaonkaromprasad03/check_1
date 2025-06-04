#Code_1

ADS_LAB_1 – Working of hash function()
Q.1 Demonstrate the working of Hash function for the following data: Integer, float, character, string, tuple, list
Q.2 Take a number list and check the hash index assigned to the set of values of the list.
Q.3 Take any expression of your choice (e.g. hello world), and find the hash value assigned to the expression using ord()function used to get the ordinal value of any character.
Q.4 Demonstrate that Mutable objects like lists, dictionaries, and sets cannot be hashed with the hash() function.
Answer. :





import hashlib
from collections import defaultdict
import random
import string
import builtins

# Hashing an integer
print(hash(50))

# Hashing a float
print(hash(8.52))

# Hashing a character
print(hash("c"))

# Hashing a string
print(hash("Birla"))

# Hashing a tuple
print(hash((10, 20, 30)))

# Trying to hash a list
try:
    print(hash([1, 2, 3]))
except TypeError as e:
    print(e)

# Demonstrating working of hash() by initializing objects
int_val = 4
str_val = 'Birla College'
flt_val = 24.56
# Printing the hash values.
# Observe that Integer value does not change but others they change
print("The integer hash value is : " + builtins.str(hash(int_val))) # Use builtins.str to avoid conflict
print("The string hash value is : " + builtins.str(hash(str_val)))  # Use builtins.str to avoid conflict
print("The float hash value is : " + builtins.str(hash(flt_val)))   # Use builtins.str to avoid conflict

# To check the hash index assigned to the set of values{11,12,13,14,15}
num=[11,12,13,14,15,16]
print('The keys assigned are:')
for i in num:
    print(hash(i),i%10)
    #k=i%10
    #print(k)

# Hashing the string data
string_to_hash = 'hello world' # Renamed variable 'str' to 'string_to_hash'
print(string_to_hash)

# We know that Hashing is the concept of converting data of arbitrary size into data of fixed size.
# We are going to use this to turn strings (or possibly other data types) into integers.
# To hash the expression hello world, that is, to get a numeric value that represents the string, we use the ord() function to get the ordinal value of any character.
# For example, the ord('f') function gives 102.

# To find the individual ordinal numbers for the characters of str :{h, e, l, o, w, r, d}
for c in string_to_hash: # Use the new variable name
    print('ordinal no for',c, 'is', ord(c))

# To get the hash of the whole string, we could just sum the ordinal numbers of each character in the string
print('hash value of',string_to_hash, 'is', sum(map(ord, string_to_hash))) # Use the new variable name

# Finding the hash keys for strings {'ab', 'cd', 'efg'}
str1=['ab','cd','efg']
for s in str1:
  print('hash value of',s,'is',hash(s))

# To get the hash value for tuples
# initializing objects
# (1) tuples: tuple are immutable
tuple_val = (1, 2, 3, 4, 5)
# Printing the hash values.
print("The tuple hash value is : " + builtins.str(hash(tuple_val)))

# (2) lists: list are mutable
list_val = [1, 2, 3, 4, 5]
# Notice exception when trying
# print("The list hash value is :" +str(hash(list_val)))
# to convert mutable object
print("Lists are unhashable, their contents can be converted to tuple representation")
# Alternatively, you can hash a tuple version of the list:
print(tuple(list_val))
print("The hash value of a tuple version of the list is : " + builtins.str(hash(tuple(list_val))))
#print("The list hash value is : " +str(hash(list_val)))

# hash() for immutable tuple of string object
tup_var = ('G','E','E','K')
print(hash(tup_var))






#Code_2

ADS_LAB_2 – Generation of Hash Tables and Open Addressing Collision Resolution

Q.1 Write and execute the Python code to create a (i) Simple Hash Table (ii) Hash Table with Collision.
Q. 2 Create a Hash Table with Collision and Demonstrate following different Open Addressing types
Collision Handling Technique
1. Probing :
a. Linear Probing
b. Quadratic Probing
2. Double Hashing
Answer.:
# Importing the required resources
import hashlib
from collections import defaultdict
import random
import string
# Generation of Simple Hash Table with Collision
class HashTable:
    def __init__(self, size):
        self.size = size
        # self.table = [None] * size  # Empty hash table
        self.table = [[] for _ in range(size)]
        # Using separate chaining for collision handling

    def hash_function(self, key):
        """Simple hash function using modulo operation"""
        return key % self.size

    def insert(self, key, value):
        """Insert key-value pair into the hash table"""
        index = self.hash_function(key)
        self.table[index].append((key, value))
        # Append to handle collisions (Separate Chaining)
        print(f"Inserted ({key}, {value}) at index {index}")

    def display(self):
        """Display the hash table contents"""
        for i, bucket in enumerate(self.table):
            print(f"Index {i}: {bucket}")

# Create a Simple Hash Table with n slots and without collision
simple_hash_table = HashTable(7)  # Hash table with 7 slots
# Insert the elements
simple_hash_table.insert(14, "land")
simple_hash_table.insert(22, "Banana")
simple_hash_table.insert(2, "Cherry")
simple_hash_table.insert(12, "Date")
simple_hash_table.insert(10, "Elderberry")
simple_hash_table.insert(11, "Guava")
simple_hash_table.insert(13, "Blueberry")

# Display the Simple hash table with no collission
print("\n Simple Hash Table Content:")
simple_hash_table.display()

# Create a Hash Table with n slots
hash_table = HashTable(7)  # Hash table with 7 slots
# Inserting values, some keys will collide if they have the same remainders
# on divsion by the size = 7
print("\nInsert the values in the hash table to create collision:")
hash_table.insert(14, "land")
hash_table.insert(21, "Banana")
hash_table.insert(2, "Cherry")
hash_table.insert(12, "Date")
hash_table.insert(9, "Elderberry")
hash_table.insert(11, "Guava")
hash_table.insert(8, "Blueberry")
# Try more values to full utlization of Hash Table of size 7
hash_table.insert(10, "Apple")
hash_table.insert(20, "Muskmellon")
hash_table.insert(30, "Orange")
hash_table.insert(17, "Rasberry")
# Display the hash table showing collisions
print("\nHash Table Content showing collision:")
hash_table.display()
# 1. Linear Probing
class LinearProbingHashTable:
    def __init__(self, size):
        self.size = size
        self.table = [None] * size  # Empty hash table

    def hash_function(self, key):
        return key % self.size  # Simple modulus hash function

    def insert(self, key, value):
        index = self.hash_function(key)
        original_index = index
        i = 0
        # Linear Probing: Find next available slot
        while self.table[index] is not None:
            print(f"Collision at index {index} for key {key}, trying next vacant slot...")
            i = i+1
            index = (original_index + i) % self.size  # important formula - Linear probing

        self.table[index] = (key, value)
        print(f"Inserted ({key}, {value}) at index {index}")

    def display(self):
        print("\nLinear Probing Hash Table:")
        for i, entry in enumerate(self.table):
            print(f"Index {i}: {entry}")

# Demonstration of Linear Probing
linear_hash_table = LinearProbingHashTable(7)
print("\n--- Linear Probing Demonstration ---")
linear_hash_table.insert(10, "Apple")  # 10 % 7 = 3
linear_hash_table.insert(20, "Banana") # 20 % 7 = 6
linear_hash_table.insert(30, "Cherry") # 30 % 7 = 2
linear_hash_table.insert(17, "Date")   # 17 % 7 = 3
print('Collision, resolved via linear probing')
print('and 17 is inserted at the vacant slot with index 4')
linear_hash_table.display()
print('\n\nCheck Linear Probing by adding 37 where 37 % 7 = 2, colliding with 30')
linear_hash_table.insert(37, "Guava")
print('Collision, resolved via linear probing')
print('and 37 is inserted at the vacant slot with index 5')
linear_hash_table.display()
print('\n\nAdd more elements say 48 to demonstarte how initial vacant slots are utlized')
linear_hash_table.insert(48, "Orange")
print('Collision, resolved via linear probing')
print('and 48 is inserted at the vacant slot with index 0')
linear_hash_table.display()
print('\n\nAdd more elements say 48 to demonstarte how initial vacant slots are utlized')
linear_hash_table.insert(19, "Watermellon")
print('Collision, resolved via linear probing')
print('and 19 is inserted at the vacant slot with index 1')
linear_hash_table.display()

# 2. Quadratic Probing
class QuadraticProbingHashTable:
    def __init__(self, size):
        self.size = size
        self.table = [None] * size  # Empty hash table

    def hash_function(self, key):
        return key % self.size  # Simple modulus hash function

    def insert(self, key, value):
        print('Inserted key =',key)
        index = self.hash_function(key)
        original_index = index
        print('original_index of ',key,'=',original_index)

        i = 0 # i is re-intialized for collision resolution
        # Quadratic Probing: Find next available slot
        while self.table[index] is not None:
            print(f"Collision at index {index} for key {key}, trying next slot using quadratic probing...")
            i = i+1
            print('i =',i)
            print('i square =',i**2)
            index = (original_index + i**2) % self.size  # Quadratic probing
            print('index = original_index + i square =',index)
        self.table[index] = (key, value)
        print(f"Inserted ({key}, {value}) at vacant index {index}")

    def display(self):
        print("\nQuadratic Probing Hash Table:")
        for i, entry in enumerate(self.table):
            print(f"Index {i}: {entry}")

# Demonstration of Quadratic Probing
quadratic_hash_table = QuadraticProbingHashTable(7) # numbered from 0 to 6
print("\n--- Quadratic Probing Demonstration ---")
quadratic_hash_table.insert(10, "Apple")  # 10 % 7 = 3
quadratic_hash_table.insert(20, "Banana") # 20 % 7 = 6
quadratic_hash_table.insert(30, "Cherry") # 30 % 7 = 2
quadratic_hash_table.insert(17, "Date")   # 17 % 7 = 3
print('Collision, resolved via quadratic probing\n')
quadratic_hash_table.display()
print('Collision, resolved via quadratic probing\n')
quadratic_hash_table.insert(27, "Elderberry") # 27 % 7 = 6 (Collision, resolved via quadratic probing)
quadratic_hash_table.display()
print('Collision, resolved via quadratic probing\n')
quadratic_hash_table.insert(44, "Papaya")
# 44 % 7 = 2 (Collision, resolved via quadratic probing)
quadratic_hash_table.display()
print('Collision, resolved via quadratic probing\n')

#Double hashing
class DoubleHashingHashTable:
    def __init__(self, size):
        self.size = size
        self.table = [None] * size  # Empty hash table
        self.prime = self.get_smaller_prime()
        # Prime number for secondary hashing will be received from the next functions
        print('self.prime =', self.prime)

    def get_smaller_prime(self):
        """Finds the largest prime smaller than table size for secondary hashing"""
        for num in range(self.size - 1, 1, -1):
          # range(start, stop, step)
            if self.is_prime(num):
                return num
        return 3  # Default to 3 if no prime found

    def is_prime(self, num):
        print('num =',num)
        """Check if a number is prime"""
        if num < 2:
            return False
        for i in range(2, int(num ** 0.5) + 1):
          # Using Sieve Principal - we need not check for the factors upto n, instead we can check upto n/2
            if num % i == 0:
                return False
        return True

    def primary_hash(self, key):
        """Primary hash function"""
        return key % self.size

    def secondary_hash(self, key):
        """Secondary hash function for step size"""
        return self.prime - (key % self.prime)

    def insert(self, key, value):
        """Insert a key-value pair using double hashing"""
        print('key =',key)
        index = self.primary_hash(key)
        print('index =',index)
        print('self.prime = ', self.prime)
        print('self.prime - (key % self.prime) =',self.prime - (key % self.prime))
        print('self.secondary_hash(key) = ', self.secondary_hash(key))
        step_size = self.secondary_hash(key)
        print('step_size =',step_size)
        original_index = index
        print('original_index =',original_index)
        i = 0

        # Double Hashing: Find next available slot
        while self.table[index] is not None:
            print(f"Collision at index {index} for key {key}, using double hashing...")
            i += 1
            print('i = ',i)
            index = (original_index + i * step_size) % self.size
            print('original_index =',original_index)
            print('step_size =',step_size)
            print('index =',index)
        self.table[index] = (key, value)
        print(f"Inserted ({key}, {value}) at index {index}")

    def display(self):
        """Display the hash table"""
        print("\nDouble Hashing Hash Table:")
        for i, entry in enumerate(self.table):
            print(f"Index {i}: {entry}")

# Demonstration of Double Hashing
double_hash_table = DoubleHashingHashTable(7)
print('self.size =',7)

print("\n--- Double Hashing Demonstration ---")
double_hash_table.insert(10, "Apple")  # 10 % 7 = 3
double_hash_table.insert(20, "Banana") # 20 % 7 = 6
double_hash_table.insert(30, "Cherry") # 30 % 7 = 2
double_hash_table.insert(17, "Date")   # 17 % 7 = 3 (Collision, resolved via double hashing)
double_hash_table.insert(27, "Elderberry") # 27 % 7 = 6 (Collision, resolved via double hashing)
double_hash_table.display()









#Code_3

ADS_LAB_3 – Separate Chaining for Collision Handling in Hash Tables 
Q.1 Create a Hash Table with Collision and Demonstrate how Separate Chaining is used for Collision Handling.
Answers.:
# Importing the required resources
import hashlib
from collections import defaultdict
import random
import string

class HashNode:
    """A node structure for linked list used in separate chaining"""
    def __init__(self, key, value):
        self.key = key
        self.value = value
        self.next = None  # Pointer to next node

class SeparateChainingHashTable:
    def __init__(self, size):
        self.size = size
        self.table = [None] * size  # Hash table with linked lists

    def hash_function(self, key):
        """Computes hash index"""
        return key % self.size

    def insert(self, key, value):
        """Inserts a key-value pair into the hash table"""
        index = self.hash_function(key)
        new_node = HashNode(key, value)

        if self.table[index] is None:
            self.table[index] = new_node  # Insert at empty index
        else:
            # Insert at the head of the linked list (for simplicity)
            current = self.table[index]
            while current:
                if current.key == key:
                    current.value = value  # Update value if key exists
                    return
                if current.next is None:
                    break
                current = current.next
            current.next = new_node  # Append new node at end
        print(f"Inserted ({key}, {value}) at index {index}")

    def search(self, key):
        """Searches for a key in the hash table"""
        index = self.hash_function(key)
        current = self.table[index]

        while current:
            if current.key == key:
                return current.value  # Key found
            current = current.next
        return None  # Key not found

    def delete(self, key):
        """Deletes a key from the hash table"""
        index = self.hash_function(key)
        current = self.table[index]
        prev = None

        while current:
            if current.key == key:
                if prev:
                    prev.next = current.next  # Remove from linked list
                else:
                    self.table[index] = current.next  # Remove first node
                print(f"Deleted key {key} from index {index}")
                return
            prev = current
            current = current.next
        print(f"Key {key} not found")

    def display(self):
        """Displays the hash table"""
        print("\nSeparate Chaining Hash Table:")
        for i, node in enumerate(self.table):
            print(f"Index {i}:", end=" ")
            current = node
            while current:
                print(f"({current.key}, {current.value}) ->", end=" ")
                current = current.next
            print("None")


# Demonstration of Separate Chaining
hash_table = SeparateChainingHashTable(5)  # Table size 5

print("\n--- Separate Chaining Demonstration ---")
hash_table.insert(10, "Apple")  # 10 % 5 = 0
hash_table.insert(20, "Banana") # 20 % 5 = 0 (Collision, separate chaining)
hash_table.insert(30, "Cherry") # 30 % 5 = 0 (Collision, separate chaining)
hash_table.insert(12, "Date")   # 12 % 5 = 2
hash_table.insert(7, "Elderberry") # 7 % 5 = 2 (Collision, separate chaining)

hash_table.display()

# Searching
print("\nSearching for key 12:", hash_table.search(12))
print("Searching for key 100:", hash_table.search(100))

# Deletion
hash_table.delete(20)
hash_table.delete(100)  # Non-existent key
hash_table.display()


#OR

# ALTERNATE_WAY_SEPERATE CHAINING
class Node:
    """A node in the linked list for separate chaining"""
    def __init__(self, key, value):
        self.key = key
        self.value = value
        self.next = None  # Pointer to the next node

class HashTable:
    """Hash Table using Separate Chaining"""
    def __init__(self, size):
        self.size = size
        self.table = [None] * size  # Array of linked lists

    def hash_function(self, key):
        """Hash function using modulo operation"""
        return key % self.size

    def insert(self, key, value):
        """Insert key-value pair into the hash table"""
        index = self.hash_function(key)
        new_node = Node(key, value)

        # If the bucket is empty, insert node
        if self.table[index] is None:
            self.table[index] = new_node
        else:
            # Insert at the beginning of the linked list (or traverse to the end)
            current = self.table[index]
            while current.next:
                if current.key == key:  # Update existing key
                    current.value = value
                    return
                current = current.next
            current.next = new_node
        print(f"Inserted ({key}, {value}) at index {index}")

    def search(self, key):
        """Search for a value by key"""
        index = self.hash_function(key)
        current = self.table[index]

        while current:
            if current.key == key:
                return current.value
            current = current.next
        return None

    def delete(self, key):
        """Delete a key-value pair from the hash table"""
        index = self.hash_function(key)
        current = self.table[index]
        prev = None

        while current:
            if current.key == key:
                if prev:
                    prev.next = current.next  # Remove node from linked list
                else:
                    self.table[index] = current.next  # Remove head node
                print(f"Deleted ({key}) from index {index}")
                return
            prev, current = current, current.next

        print(f"Key {key} not found!")

    def display(self):
        """Display the hash table"""
        print("\nSeparate Chaining Hash Table:")
        for i, node in enumerate(self.table):
            print(f"Index {i}:", end=" ")
            current = node
            while current:
                print(f"({current.key}, {current.value}) ->", end=" ")
                current = current.next
            print("None")

# Demonstration of Separate Chaining
hash_table = HashTable(7)

print("\n--- Separate Chaining Demonstration ---")
hash_table.insert(10, "Apple")  # 10 % 7 = 3
hash_table.insert(20, "Banana") # 20 % 7 = 6
hash_table.insert(30, "Cherry") # 30 % 7 = 2
hash_table.insert(17, "Date")   # 17 % 7 = 3 (Collision, linked list formed)
hash_table.insert(27, "Elderberry") # 27 % 7 = 6 (Collision, linked list formed)

hash_table.display()

# Searching
print("\nSearching for key 17:", hash_table.search(17))  # Expected output: Date
print("Searching for key 5:", hash_table.search(5))  # Expected output: None

# Deletion
hash_table.delete(17)
hash_table.display()






#Code_4


ADS_LAB_4 – Rehashing and Universal Hashing 
Q.1 Create a Hash Table and Demonstrate how the following Hashing Techniques are performed
1.	Rehashing
2.	Universal Hashing



class RehashingHashTable:
    def __init__(self, size):
        self.size = size
        self.count = 0  # Track the number of elements
        self.table = [None] * size
        self.load_factor_threshold = 0.7  # Rehash when load factor exceeds 0.7

    def hash_function(self, key):
        """Primary hash function"""
        return key % self.size

    def insert(self, key, value):
        """Insert key-value pair, with rehashing when necessary"""
        if self.count / self.size > self.load_factor_threshold:
            print("\nLoad factor exceeded! Rehashing table...")
            self.rehash()

        index = self.hash_function(key)
        original_index = index
        i = 0

        # Linear Probing for Collision Resolution
        while self.table[index] is not None:
            print(f"Collision at index {index} for key {key}, trying next slot...")
            i += 1
            index = (original_index + i) % self.size  # Linear probing

        self.table[index] = (key, value)
        self.count += 1
        print(f"Inserted ({key}, {value}) at index {index}")

    def rehash(self):
        """Rehash by doubling the table size and reinserting elements"""
        new_size = self.get_next_prime(self.size * 2)  # Get next prime number
        old_table = self.table
        self.size = new_size
        self.table = [None] * new_size
        self.count = 0  # Reset count

        # Reinsert elements into new table
        for item in old_table:
            if item is not None:
                self.insert(item[0], item[1])

    def get_next_prime(self, num):
        """Find the next prime number greater than the given number"""
        while True:
            if self.is_prime(num):
                return num
            num += 1

    def is_prime(self, num):
        """Check if a number is prime"""
        if num < 2:
            return False
        for i in range(2, int(num ** 0.5) + 1):
            if num % i == 0:
                return False
        return True

    def display(self):
        """Display the hash table"""
        print("\nRehashing Hash Table:")
        for i, entry in enumerate(self.table):
            print(f"Index {i}: {entry}")


# Demonstration of Rehashing
rehashing_hash_table = RehashingHashTable(5)  # Initial size 5

print("\n--- Rehashing Demonstration ---")
rehashing_hash_table.insert(10, "Apple")  # 10 % 5 = 0
rehashing_hash_table.insert(20, "Banana") # 20 % 5 = 0 (Collision)
rehashing_hash_table.insert(30, "Cherry") # 30 % 5 = 0 (Collision)
rehashing_hash_table.insert(25, "Date")   # 25 % 5 = 0 (Collision, triggers rehashing)
rehashing_hash_table.insert(15, "Elderberry") # New table size used

rehashing_hash_table.display()


# Universal Hashing
import random

class UniversalHashing:
    def __init__(self, size, prime=101):  # Prime should be greater than table size
        self.size = size
        self.prime = prime
        self.a = random.randint(1, prime - 1)  # Random 'a' in range [1, p-1]
        self.b = random.randint(0, prime - 1)  # Random 'b' in range [0, p-1]
        self.table = [None] * size

    def hash_function(self, key):
        """Universal Hash Function"""
        return ((self.a * key + self.b) % self.prime) % self.size

    def insert(self, key, value):
        """Insert key-value pair into the hash table"""
        index = self.hash_function(key)
        while self.table[index] is not None:  # Handling collision (Linear Probing)
            index = (index + 1) % self.size
        self.table[index] = (key, value)
        print(f"Inserted ({key}, {value}) at index {index}")

    def display(self):
        """Display the hash table"""
        print("\nUniversal Hashing Table:")
        for i, entry in enumerate(self.table):
            print(f"Index {i}: {entry}")

# Demonstration
universal_hash_table = UniversalHashing(size=10)  # Hash table size 10

print("\n--- Universal Hashing Demonstration ---")
universal_hash_table.insert(10, "Apple")
universal_hash_table.insert(20, "Banana")
universal_hash_table.insert(30, "Cherry")
universal_hash_table.insert(17, "Date")
universal_hash_table.insert(25, "Elderberry")
universal_hash_table.display()




#Code_5

ADS_LAB_5 – Max_Heap, Binary_Heap, Priority Queue, Binomial Queue
 Q.1 Demonstrate the working of the following
 (1) Max_Heap (2) Binay Heap (3) Priority Queue (4) Binomial Queue
Answers.:
1) To create max Heap nad exceuting all its properties using the heapq module

import heapq
# Create a min-heap
heap = []
heapq.heappush(heap, 10)
heapq.heappush(heap, 5)
heapq.heappush(heap, 20)
print("Min-Heap:", heap)
print("Pop Min:", heapq.heappop(heap))  # 5
import heapq
class MaxHeap:
    def __init__(self):
        self.heap = []

    def push(self, item):
        # Invert the value for max-heap behavior
        heapq.heappush(self.heap, -item)
        print('Element Inserted =',item)

    def pop(self):
        # Invert back to get the original value
        return -heapq.heappop(self.heap) if self.heap else None

    def peek(self):
        # Return max without removing it
        return -self.heap[0] if self.heap else None

    def heapify(self, iterable):
        self.heap = [-item for item in iterable]
        heapq.heapify(self.heap)

    def replace(self, item):
        # Replace root and return old max
        return -heapq.heapreplace(self.heap, -item)

    def __str__(self):
        # Return original values
        return str([-item for item in self.heap])

# Call the function MaxHeap()
max_heap = MaxHeap()
print('Insert the elements in the Heap')
# Use a different name for the list to avoid shadowing the built-in list type
input_elements = []
print('How many elements :')
n_str = input() # Get input as a string
n = int(n_str)  # Convert the string to an integer
for i in range(n):
  print('Enter the element')
  element = int(input()) # Store the entered integer
  max_heap.push(element) # Push the element to the heap
  input_elements.append(element) # Append the element to the list for heapify

print("Heap Elements:", max_heap)
print("Peek Max:", max_heap.peek())

# Bulk heapify
max_heap.heapify(input_elements) # Pass the list of user-entered elements
print("Heapified:", max_heap)

print("Pop Max:", max_heap.pop())
print("After Pop:", max_heap)

# Replace max with new value
print("Replaced Max with 50:", max_heap.replace(50))
print("After Replace:", max_heap)

max_heap.push(70)
print("Heap Elements:", max_heap)
print("Peek Max:", max_heap.peek())

# Push then Pop efficiently
print("Perform Pop Max again:", max_heap.pop())
print("Final Heap:", max_heap)



2) To create a Binary Heap (Using heapq, with list as complete binary tree).

class MaxBinaryHeap:
    def __init__(self):
        self.heap = []

    def insert(self, item):
        self.heap.append(item)
        self._percolate_up(len(self.heap) - 1)

    def delete_max(self):
        if not self.heap:
            return None
        max_item = self.heap[0]
        last_item = self.heap.pop()
        if self.heap:
            self.heap[0] = last_item
            self._percolate_down(0)
        return max_item

    def peek_max(self):
        return self.heap[0] if self.heap else None

    def _percolate_up(self, idx):
        parent = (idx - 1) // 2
        if idx > 0 and self.heap[idx] > self.heap[parent]:
            self.heap[idx], self.heap[parent] = self.heap[parent], self.heap[idx]
            self._percolate_up(parent)

    def _percolate_down(self, idx):
        largest = idx
        left = 2 * idx + 1
        right = 2 * idx + 2

        if left < len(self.heap) and self.heap[left] > self.heap[largest]:
            largest = left
        if right < len(self.heap) and self.heap[right] > self.heap[largest]:
            largest = right
        if largest != idx:
            self.heap[idx], self.heap[largest] = self.heap[largest], self.heap[idx]
            self._percolate_down(largest)
    def build_heap(self,iterable):
      print('iterable :',iterable)
      self.heap = iterable
      for i in range(len(self.heap)):
        self._percolate_down(i)

    def __str__(self):
        return __builtins__.str(self.heap)
max_heap=MaxHeap()
heap = MaxBinaryHeap()
print('Insert the elements in the Heap')
# Use a different name for the list to avoid shadowing the built-in list type
input_elements_list = []
print('How many elements :')
num_elements_str = input() # Get input as a string
n = int(num_elements_str)  # Convert the string to an integer
for i in range(n):
  print('Enter the element')
  element = int(input()) # Store the entered integer
  heap.insert(element) # Push the element to the heap (using the MaxHeap class)
  input_elements_list.append(element) # Append the element to the list for heapify

print("Max Binary Heap:", heap)
print("Peek Max:", heap.peek_max())
print("Delete Max:", heap.delete_max())
print("After Delete:", heap)

# Build from list
# Pass the list containing the user-entered elements
heap.build_heap(input_elements_list)
print("Heapified:", heap)



import heapq
binary_heap = [9, 4, 7, 1, -2, 6, 5]
heapq.heapify(binary_heap)  # Converts list to binary heap
print("Binary Heap:", binary_heap)
print("Min Element:", heapq.heappop(binary_heap))  # Removes smallest
import heapq
class PriorityQueue:
    def __init__(self):
        self._heap = []
        self._index = 0

    def push(self, item, priority):
        heapq.heappush(self._heap, (-priority, self._index, item))
        self._index += 1

    def pop(self):
        return heapq.heappop(self._heap)[-1]

    def is_empty(self):
        return not self._heap


3) Demonstratation of Priority Queue (Using queue.PriorityQueue)


import pandas as pd
#Imports the built-in thread-safe PriorityQueue class.
from queue import PriorityQueue
#Initializes an empty priority queue.
pq = PriorityQueue()
print(pq)
#Adds an item with priority 3. The queue maintains items in ascending order.
pq.put(3)
#Adds another item with higher priority (lower number = higher priority).
pq.put(1)
pq.put(5)
pq.put(2)
print(pq.queue)
#Removes and returns the lowest-priority number, i.e., 1
print("Smallest Priority:", pq.queue, pq.get())  # 1
pd.DataFrame(pq.queue)


4) Binomial Queue (Using external library or manual minimal class)
# Create a node of a Binomial Tree
class BinomialNode:
    def __init__(self, key):
        self.key = key
        self.children = []
# Here, key: The priority or value stored and
#children: A list of other BinomialNodes that are its subtrees.
# Define a Main Priority Queue Class
class BinomialQueue:
    def __init__(self):
        self.trees = []
#The queue is represented as a list of binomial trees,
#where the index indicates the degree of the tree and only one tree of each degree is allowed (just like binary addition).
#  Insert a New Key as a single-node tree.
    def insert(self, key):
        new_tree = BinomialNode(key)
        self.trees.append(new_tree)
        self.trees.sort(key=lambda node: node.key)
# To find and remove the root of the tree with the smallest key.
    def find_min(self):
        return min((t.key for t in self.trees), default=None)

    def delete_min(self):
        if not self.trees:
            return None
        min_tree = min(self.trees, key=lambda t: t.key)
        self.trees.remove(min_tree)
        return min_tree.key
bq = BinomialQueue()
bq.insert(30)
bq.insert(10)
bq.insert(20)
bq.insert(15)
bq.insert(40)
bq.insert(56)

print("Min in Binomial Queue:", bq.find_min())  # 10
print("Deleted Min:", bq.delete_min())          # 10




#Code_6


ADS_LAB_6_0 – Graphing Algorithms – Drawing Graphs
 1.1 Create and draw a simple undirected graph, G(V, E) with set of nodes V= {1,2,3,4,5} and set of edges E={ (1, 2), (2, 3), (1, 3), (3, 4), (1, 4), (1, 5)} and add some more nodes and edges in the graph G and draw and find 
(i) Total no. of nodes. (ii) Total no. of edges. (iii) List of all nodes. (iv) Degree of all nodes. (v) List of all edges. (vi) List of all nodes from a vertex '2'. (vii) Adjacency List for the Graph G
Answers.:
# Create the objects for both pacakges - networkx, matplotlib
import networkx as nx
import matplotlib as plt
# 6.1 Create and draw a simple undirected graph with labeled nodes
G1 = nx.Graph()
# add the nodes and edges to it.
G1.add_node(1)
G1.add_node(2)
G1.add_node(3)
G1.add_node(4)
G1.add_node(5)
G1.add_edge(1, 2)
G1.add_edge(2, 3)
G1.add_edge(1, 3)
G1.add_edge(3, 4)
G1.add_edge(1, 4)
G1.add_edge(1, 5)
# nx.draw(G1) creates the graph without the labels.
# Hence we use draw_networkx(), with_labels=True
nx.draw_networkx(G1, with_labels = True)
print("Total no. of nodes: ",int(G1.number_of_nodes()))
print("Total no. of edges: ",int(G1.number_of_edges()))
print("List of all nodes: ", list(G1.nodes()))
print("Degree of all nodes: ", dict(G1.degree()))
print("List of all edges: ", list(G1.edges(data=True)))
print("Adjacency list from a vertex '2': ",list(G1.neighbors(2)))





# Adding two or more edges at the same time by taking from an edgelist
G2 = nx.Graph()
elist = [(1, 2), (2, 3), (1, 3), (3, 4), (1, 4), (1, 5),(2,6),(3,6)]
G2.add_edges_from(elist)
nx.draw_networkx(G2, with_labels = True)
print("Total no. of nodes: ",int(G2.number_of_nodes()))
print("Total no. of edges: ",int(G2.number_of_edges()))
print("List of all nodes: ", list(G2.nodes()))
print("Degree of all nodes: ", dict(G2.degree()))
print("List of all edges: ", list(G2.edges(data=True)))
print("List of all nodes from a vertex '2': ",list(G2.neighbors(2)))
print("Adjacency List for the Graph")
for i in list(G2.nodes()):
    print(i)
    print(list(G2.neighbors(i)))


1.2 Create and draw a simple directed graph, G (V, E) with set of nodes V= {1,2,3,4,5} and set of edges E={ (1, 2), (2, 3), (1, 3), (3, 4), (1, 4), (1, 5)} and add some more nodes and edges in the graph G and find 
(i) Total no. of nodes. (ii) Total no. of edges.  (iii) List of all nodes. (iv) Degree of all nodes. 
(v) List of all edges.  (vi) List of all nodes from a vertex '2'.  (vii) Adjacency List for the Graph 
Answer.:
# 6.2 Create and draw a simple directed graph using Digraph() with labeled nodes
G3 = nx.DiGraph()
elist1 = [(1, 2), (2, 3), (1, 3), (3, 4), (1, 4), (1, 5)]
G3.add_edges_from(elist1)
nx.draw_networkx(G3, with_labels = True)
print("Total no. of nodes: ",int(G3.number_of_nodes()))
print("Total no. of edges: ",int(G3.number_of_edges()))
print("List of all nodes: ", list(G3.nodes()))
print("Degree of all nodes: ", dict(G3.degree()))
print("List of all edges: ", list(G3.edges(data=True)))
print("List of all nodes from a vertex '2': ",list(G3.neighbors(2)))
print("Adjacency List for the Graph")
for i in list(G3.nodes()):
    print(i)
    print(list(G3.neighbors(i)))


1.3 Create and draw a weighted undirected graph with labeled nodes and weighted edges, by adding two or more edges with weights at the same time and find 
(i) Total no. of nodes. (ii) Total no. of edges. (iii) List of all nodes. (iv) Degree of all nodes. (v) List of all edges. (vi) List of all nodes from a vertex '2'. (vii) Adjacency List for the Graph G
Answer.:
# 6.3 Create and draw a weighted undirected graph with labeled nodes and weigthed edges,
# by adding two or more edges with weights at the same time.
import networkx as nx
G_wted = nx.Graph()
elist = [('a', 'b', 5.0), ('b', 'c', 3.0), ('a', 'c', 1.0), ('c', 'd', 7.3)]
G_wted.add_weighted_edges_from(elist)
pos=nx.spring_layout(G_wted)
edge_label=nx.get_edge_attributes(G_wted,"weight")
nx.draw_networkx(G_wted,pos=pos)
nx.draw_networkx_edge_labels(G=G_wted,pos=pos,edge_labels=edge_label)


1.4 Create and draw a weighted directed graph with labeled nodes and weighted edges, by adding two or more edges with weights at the same time and find
 (i) Total no. of nodes. (ii) Total no. of edges. (iii) List of all nodes. (iv) Degree of all nodes. (v) List of all edges. (vi) List of all nodes from a vertex '2'. (vii) Adjacency List for the Graph G
Answer.:
# 6.4 Create and draw a weighted directed graph with labeled nodes and weigthed edges,
# by adding two or more edges with weights at the same time.
import matplotlib as plt
plt.pyplot.figure(figsize=(14,8)) # increasing size of the figure
G = nx.DiGraph() #directed graph
elist = [('a', 'b', 4), ('b', 'c', 2), ('c', 'd', 2), ('d', 'f', 2),
         ('e', 'f', 5), ('e', 'b', 3), ('c', 'e', 1), ('e','a',6)]
G.add_weighted_edges_from(elist)
pos=nx.spring_layout(G)
nx.draw_networkx(G,pos)
label=nx.get_edge_attributes(G,'weight')
nx.draw_networkx_edge_labels(G,pos,edge_labels=label)
print("Total no. of nodes: ",int(G.number_of_nodes()))
print("Total no. of edges: ",int(G.number_of_edges()))
print("List of all nodes: ", list(G.nodes()))
print("Degree of all nodes: ", dict(G.degree()))
print("List of all edges: ", list(G3.edges(data=True)))
print("List of all nodes from a vertex 'b': ",list(G.neighbors('b')))
print("Adjacency List for the Graph")
for i in list(G.nodes()):
    print(i)
    print(list(G.neighbors(i)))



1.5 Create and draw a undirected multigraph with labeled nodes and weighted edges, by adding two or more edges with weights at the same time and find
 (i) Total no. of nodes. (ii) Total no. of edges. (iii) List of all nodes. (iv) Degree of all nodes. (v) List of all edges. (vi) List of all nodes from a vertex 'b'. (vii) Adjacency List for the Graph G Answers.:
# 6.5 Create and Draw a simple undirected graph and display its nodes and edges using dictionary
graph={'a':['c'],'b':['c','e'],'c':['a','b','d','e'],'d':['c'],'e':['b','c'],'f':[] }
# returns the edges
def generate_edges(graph):
    edges = []
    for node in graph:
        for neighbour in graph[node]:
            edges.append((node, neighbour))
    return edges
print(generate_edges(graph))
# returns a list of isolated nodes.
def find_isolated_nodes(graph):
    isolated = []
    for node in graph:
        if not graph[node]:
          isolated+= node
    return isolated
print(find_isolated_nodes(graph))


1.6 Create and draw (i) Complete graph (ii) Cyclic graph (iii) bipartite graph.

Answer.:
#  Create and draw different types of graphs: Complete graph, cyclic graph, bipartite graph
#COMPLETE GARPH
G1 = nx.complete_graph(6)
nx.draw_networkx(G1,with_labels=True)
# CYCLIC GRAPH
G2 = nx.cycle_graph(6)
nx.draw_networkx(G2,with_labels=True)





ADS_LAB_6_1 – Graphing Algorithms – DRAWING MINIMAL SPANNING TREE USING KRUSKAL AND PRIM'S ALGORITHM 
Implement Kruskal and Prim's Algorithm for
 (a) any cycle G (b) any random graph. (c) graphs discussed in class
 
Answer.:
# LAB (6) - GRAPH ALGORITHMS
# LAB (6_1) - DRAWING MINIMAL SPANNING TREE USING KRUSKAL AND PRIM'S ALGORITHM
import matplotlib.pyplot as plt
import networkx as nx
from networkx.algorithms import tree
# (a) First try with any cycle G, since for for a tree Graph should be connected and acyclic
# (b) Implement Kruskal and Prim's Algorithm for any random graph.
# (c) Implement Kruskal and Prim's Algorithm for graphs discussed in class
G = nx.cycle_graph(4)
G.add_edge(0, 3, weight=2)
mst = tree.minimum_spanning_edges(G, algorithm="kruskal", data=False)
edgelist = list(mst)
sorted(sorted(e) for e in edgelist)
print(edgelist)
nx.draw_networkx(G)
G = nx.cycle_graph(4)
G.add_edge(0, 3, weight=2)
mst = tree.minimum_spanning_edges(G, algorithm="prim", data=False)
edgelist = list(mst)
print(edgelist)
sorted(sorted(e) for e in edgelist)
nx.draw_networkx(G)
G = nx.Graph()
G.add_nodes_from(['a', 'b', 'c', 'd', 'e','z'])
G.add_edge('a', 'b', weight=4)
G.add_edge('a', 'c', weight=2)
G.add_edge('b', 'd', weight=5)
G.add_edge('b', 'c', weight=1)
G.add_edge('c', 'd', weight=8)
G.add_edge('c', 'e', weight=10)
G.add_edge('d', 'e', weight=2)
G.add_edge('d', 'z', weight=6)
G.add_edge('e', 'z', weight=3)
#pos (dictionary, optional) – A dictionary with nodes as keys and positions as values.
# If not specified a spring layout positioning will be computed.
pos=nx.spring_layout(G,iterations=50)
#iterations=50
# nx.draw_networkx(G)
nx.draw_networkx(G,pos)
label=nx.get_edge_attributes(G,'weight')
nx.draw_networkx_edge_labels(G,pos,edge_labels=label)
# Draw Minimal Spanning Tree
mst = tree.minimum_spanning_edges(G, algorithm="prim", data=False)
print(mst)
edgelist = list(mst)
print(sorted(sorted(e) for e in edgelist))



ADS_LAB_6_2 – Graphing Algorithms – DIJKASTRA SHORTEST PATH ALGORITHM 
(a) Implement with the graphs discussed in class as below
 
Answer.:
# LAB (6) - GRAPH ALGORITHMS
# LAB (6_2) - DIJKASTRA SHORTEST PATH ALGORITHM
import networkx as nx
import matplotlib as plt
# (a) Implement with the graphs discussed in class
# (b) Implement it with any arbitrary graph
G = nx.Graph()
G.add_nodes_from(['a', 'b', 'c', 'd', 'e','z'])
G.add_edge('a', 'b', weight=4)
G.add_edge('a', 'c', weight=2)
G.add_edge('b', 'd', weight=5)
G.add_edge('b', 'c', weight=1)
G.add_edge('c', 'd', weight=8)
G.add_edge('c', 'e', weight=10)
G.add_edge('d', 'e', weight=2)
G.add_edge('d', 'z', weight=6)
G.add_edge('e', 'z', weight=3)
#pos (dictionary, optional) – A dictionary with nodes as keys and positions as values.
# If not specified a spring layout positioning will be computed.
pos=nx.spring_layout(G,iterations=50)
#iterations=50
# nx.draw_networkx(G)
nx.draw_networkx(G,pos,with_labels=True)
label=nx.get_edge_attributes(G,'weight')
nx.draw_networkx_edge_labels(G,pos,edge_labels=label)
print(nx.dijkstra_path(G,'a','z'))
print(nx.dijkstra_path_length(G,'a','z'))
# Take edges from edge list
import networkx as nx
G_wted = nx.Graph()
elist = [('a', 'b', 4), ('a', 'c', 2), ('b', 'd', 5), ('b', 'c', 1), ('c', 'd', 8), ('c', 'e', 10),
         ('d', 'e', 2), ('d', 'z', 6), ('e', 'z', 3)]
G_wted.add_weighted_edges_from(elist)
pos=nx.spring_layout(G_wted)
edge_label=nx.get_edge_attributes(G_wted,"weight")
nx.draw_networkx(G_wted,pos=pos)
nx.draw_networkx_edge_labels(G=G_wted,pos=pos,edge_labels=edge_label)
print(nx.dijkstra_path(G_wted,'a','z'))
print(nx.dijkstra_path_length(G_wted,'a','z'))












#Code_7

ADS_LAB_7_1: Simple Union-Find without optimization 
Q.1 Create 5 disjoint sets and demonstrate how Simple Union-Find without optimization happens and display the final root for each node.
# Simple Union-Find without optimization
class DisjointSetSimple:
    def __init__(self, n):
        self.parent = list(range(n))
    def find(self, x):
        while self.parent[x] != x:
            x = self.parent[x]
        return x
    def union(self, x, y):
        xroot = self.find(x)
        print('xroot = ',xroot)
        yroot = self.find(y)
        print('yroot = ',yroot)
        if xroot != yroot:
            self.parent[xroot] = yroot
    def print_sets(self):
        for i in range(len(self.parent)):
          print(f"{i} -> {self.find(i)}")
ds=DisjointSetSimple(5)
ds.parent
ds.union(0, 1)
ds.find(0), ds.find(1)
ds.parent
ds.find(1)
ds.parent
ds.find(2)
ds.union(1,2)
ds.parent
# Now check roots:
ds.find(0)
ds.find(3)
ds.union(2,3)
ds.find(3)
ds.parent
ds.find(3)
ds.union(3,4)
ds.find(3)
ds.parent
ds.print_sets()



ADS_LAB_7_2: Smart Union-Find without optimization 
Q.1 Create 5 disjoint sets and demonstrate how Smart Union-Find without optimization happens and display the final root for each node.
Answer.:
class DisjointSetSmart:
    def __init__(self, n):
        # Initialize parent to self and rank to 0
        self.parent = list(range(n))
        self.rank = [0] * n

    def find(self, x):
        #Find with Path Compression
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])  # path compression
        return self.parent[x]

    def union(self, x, y):
        #Union by Rank
        xroot = self.find(x)
        yroot = self.find(y)
        if xroot == yroot:
            return
        # attach smaller rank tree under root of higher rank tree
        if self.rank[xroot] < self.rank[yroot]:
            self.parent[xroot] = yroot
        elif self.rank[xroot] > self.rank[yroot]:
            self.parent[yroot] = xroot
        else:
            self.parent[yroot] = xroot
            self.rank[xroot] += 1

    def print_sets(self):
        # For debugging: shows the current root for each element
        print("Element -> Root mapping:")
        for i in range(len(self.parent)):
            print(f"{i} -> {self.find(i)}")
# Create 5 disjoint sets
ds = DisjointSetSmart(5)
# Check the roots of each set
ds.find(0), ds.find(1), ds.find(2), ds.find(3), ds.find(4)
ds.parent
ds.union(0, 1)
ds.parent
ds.union(1, 2)
ds.parent
ds.union(3, 4)
ds.parent
ds.print_sets()
# Should show all in group 0-1-2 as one set, 3-4 as another





#Code_8


ADS_LAB_8: String – Pattern Matching Algorithms Take a text and a string pattern and demonstrate the working of following Algorithms
1.	Naive String Matching Algorithm
2.	Rabin-Karp Algorithm
3.	The Knuth-Morris-Pratt (KMP) Algorithm

Answer.:
1. Naive String Matching Algorithm

def naive_string_match(text, pattern):
    n = len(text)
    m = len(pattern)
    for i in range(n - m + 1):
        match = True
        for j in range(m):
            if text[i + j] != pattern[j]:
                match = False
                break
        if match:
            print(f"Pattern found at index {i}")
#Test Case
text = "ABCABCABC"
pattern = "CAB"
naive_string_match(text, pattern)


Output.:
 



2. Rabin-Karp Algorithm

def rabin_karp(text, pattern, q=101):
    d = 256  # number of characters in alphabet
    n = len(text)
    m = len(pattern)
    h = pow(d, m-1) % q
    p = 0  # hash for pattern
    t = 0  # hash for text window
    # Initial hash computation
    for i in range(m):
        p = (d * p + ord(pattern[i])) % q
        t = (d * t + ord(text[i])) % q
    for i in range(n - m + 1):
        if p == t:  # possible match
            if text[i:i + m] == pattern:
                print(f"Pattern found at index {i}")
        if i < n - m:
            t = (d * (t - ord(text[i]) * h) + ord(text[i + m])) % q
            if t < 0:
                t += q
#### Test Case
text = "DO DIE DO"
pattern = "DO"
rabin_karp(text, pattern)

Output.:
 






3. The Knuth-Morris-Pratt (KMP) Algorithm
def compute_lps(pattern):
    lps = [0] * len(pattern)
    length = 0
    i = 1
    while i < len(pattern):
        if pattern[i] == pattern[length]:
            length += 1
            lps[i] = length
            i += 1
        else:
            if length != 0:
                length = lps[length - 1]
            else:
                lps[i] = 0
                i += 1
    return lps

def kmp_search(text, pattern):
    n = len(text)
    m = len(pattern)
    lps = compute_lps(pattern)
    i = j = 0

    while i < n:
        if pattern[j] == text[i]:
            i += 1
            j += 1
        if j == m:
            print(f"Pattern found at index {i - j}")
            j = lps[j - 1]

        elif i < n and pattern[j] != text[i]:
            if j != 0:
                j = lps[j - 1]
            else:
                i += 1
# Test Case
text = "ABABDABACDABABCABAB"
pattern = "ABABCABAB"
kmp_search(text, pattern)
OutPut.:
 


