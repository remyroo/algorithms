1. Hashing tables
    - A hash table is a data structure made of two parts: an array where all the data is stored, and a hash function/algorithm that determines where new data goes (i.e. into which hash bucket) and how to retrieve existing data from the array.
    - A hash table is only as good as its hash function. A hash function should be:
      1. Simple to compute to maximize on efficency.
      2. Consistent i.e the same value will always return the same key. If it was inconsistent then you wouldn’t be able to retrieve stored values later.
      3. Minimize collisions as much as possible.
    - Collisions are when your hash function tries to place more than one element into a single hash bucket. This is inevitable since your data set will likely always be larger than your hash table. So you’re aiming for a “well-spaced” table where the full range of the table is used and there is an equal distribution of items in each hash bucket rather than having too many items in just one hash bucket which would then be inefficient to search through. Here are some ways to manage collisions:
      1. Linear probing: assuming your hash buckets can each only hold one element, if a new element is meant to go into a hash bucket that’s already occupied, the hash function will just probe down the array of hash buckets till it finds an empty one. The downside is this can lead to clustering (placing a majority of data into only a few hash buckets rather than spacing them out and/or having most of the data in one section of the hash table rather than spread out over the whole range).
      2. Chaining: instead of each hash bucket holding a single element, it holds a linked list! So when a collision happens, you just apppend the new element onto the front of the linked list, enabling the bucket to hold multiple elements. The lookup time of the linked list is `O(n)` so to ensure this doesn’t get slow, the hash function still has to do a great job of spreading out the elements and avoiding collisions so that the linked lists never get to large, and the overall lookup time can remain as close to `O(1)` as possible.
    - Big(O):
      - Other linear data structures like an array or linked list requires you to begin at the first node/index and look at each element sequentially to find the one you’re looking for. Worst-case scenario, the element is at the end of the array. So time complexity increases with the number of elements i.e. `O(n)`. If it’s a sorted array you could use binary search which gives you the slightly better `O(log n)`.
      But a hash table basically creates a mapping of data of an arbitrary size to data of a fixed size. E.g (k,v) where the key is the hash bucket index (fixed size) and the value is the actual data placed there (arbitrary size). So the amount of time it takes to look up an element is always the same - you ask the hash function, it returns the location. This is constant time `O(1)` which is great! 
    - Uses: Hash tables are great for quick search, insert and delete. When not to use hash tables: if storing sorted data that you want to keep sorted or if you need to retrieve multiple items at a time.
      1. Spellchecker in a word doc processor e.g. you have a hash table containing all the words in the english dictionary. When a user types in a word, you send it to the hash function. If it returns a key, it means the word exists and is spelled correctly. If not, send an error back i.e. squiggly red lines under the word!
      2. Cryptographic hash functions: these are one-way hash functions i.e. it is impossible to determine the input value based off of the outputted hash key. They are used to create digital signatures, message authentication codes (HMACS) and other forms of authentication. Examples of these hash functions include:
        1. MD5 (message digest algorithm): turns out it’s super easy to create collisions with it, allowing brute-force attacks (get a hashed password, then continually input other guessed passwords into the algorithm till you get a hash that matches the hash you stole, revealing the original password), so it’s no longer used for crypto. Instead, it’s more commonly used to check data integrity i.e. that a file has not changed as a result of faulty file transfer, disk error or any other non-malicious error. MD5 takes a file as input and calculates a hash known as a checksum. If the file is then changed in any way, its checksum will also change.
        2. Secure Hash Functions (SHA): a family of hash functions used for cryptography. SHA-2 is most commonly used, SHA-3 is the most recent.
        3. Bcrypt: a hashing algorithm that includes a salt (unique random string placed at the beginning of the hash and known only to the originating server) and a work-factor. The work-factor is the numer of rounds of encryption your password goes through. Extending it like this means it takes longer to brute-force the password and match the hash, making it more time consuming and costly for hackers to attempt.