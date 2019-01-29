1. Insertion Sort
  - It takes an input and maintains two subsets: a sorted and unsorted subset. It iterates through the unsorted subset, inserting each individual element into the correct position in the sorted subset.
  - Big(O):
    - Time complexity: There are two loops in the code below - one to loop through the array and one to loop through the sorted subset to determine where to insert an element - meaning the time complexity is quadratic `O(n^2)`. But if the array is already partially sorted, fewer comparisons need to be made and best-case can be `O(n)`.
    - Space complexity: In-place (acts directly on the inputted data) and internal (uses main memory/RAM instead of external like disk/tape) = constant space `O(1)`.
  - Uses: better with smaller datasets.
  - Instructions:
    1. Assume first element in the array is sorted.
    2. Compare next element in the array to the sorted element to its left.
    3. Shift over any necessary elements to make room for the new element and insert it into the sorted subset.
  - Code:
    ```ruby
      def insertion_sort(a)
        (1...a.length).each do |i|
          # create subset called j
          j = i

          while j > 0 && a[j] < a[j-1]
            a[j], a[j-1] = a[j-1], a[j]
            j -= 1
          end
        end
        return a
      end
    ```

2. Merge Sort
    - It takes an input and divides it into 2 smaller subsets until it reaches the smallest collection (1 item). It then sorts the subsets before finally combining the smaller subsets into a sorted whole.
    - It is made up of two functions: `merge_sort` which recursively divides the subsets and calls `merge` which sorts and combines them.
    - Big(O):
      - Time complexity: Since we divide the elements to be searched into smaller elements, merge sort allows us to sort multiple items at the same time rather one iteratively. The recursive bit takes `O(logn)` and the process of appending the sorted elements to each other takes `O(n)`. So in total, merge sort uses linearithmic time `O(nlogn)`. Best case for partially sorted input is `O(n)`.
      - Space complexity: Out-of-place (because of the temporary array it creates to sort and append elements) and can be external = space grows as inputs grow `O(n)`
    - Uses: Ruby's sort method uses merge sort for larger arrays and insertion sort for smaller arrays.
    - Code:
      ```ruby
        def merge_sort(a)
          return a if a.length <= 1

          middle = a.length/2
          left_array = merge_sort(a[0...middle])
          right_array = merge_sort(a[middle..-1])

          merge(left_array, right_array)
        end

        def merge(left, right)
          result = []

          until left.empty? || right.empty?
            result << (left[0] <= right[0] ? left.shift : right.shift)
          end
          result + left + right
        end
      ```

3. Quick sort
    - It takes an array, selects a pivot point and partitions the array so that smaller elements are in the partition to the left of the pivot point and larger elements are in the right partition. It continues to do this recursivley until the partitions are single-element, then builds them back up again by sorting left partition, pivot point and right partition together. The pivot point is usually the first, last or median element in the array.
    - Big(O):
      - Time complexity: is dependent on:
          1. How close the pivot point is to the median. If it's not close to the median, you get a lop-sided partition forcing you to compare the pivot point to every element in the array.
          2. How sorted the data already is. The more sorted, the more likely you'll end up with lop-sided partitions.
          The time complexity for an unsorted array with a pivot point close to the median is `O(nlogn)`. For a mostly-sorted array or one with a pivot point far from the median its `O(n^2)`. Quick sort is often considered the most efficient general purpose sorting algorithm if you can control for these two things.
      - Space complexity: Unlike merge sort that creates a temporary array to store it's partitions, quick sort swaps elements, allowing it to act on the inputted array itself rather than require lots of extra space. It is in-place and internal, using less additional memory than merge sort.
      - Uses: Quick sort uses up less space than merge sort and works with internal memory but it is unstable, so if maintaining the pre-sorted order of elements is important, use merge sort. Also, use quick sort if you're sure the data will always be unsorted. Quick sort is used in Node.js sort method and browsers like V8. 
      - Code:
        ```ruby
          def quick_sort(a, left_index, right_index)
            return a if a.length <= 1

            pivot_index = partition(a, left_index, right_index)

            # if left_index has not reached pivot, keep sorting
            if left_index < pivot_index - 1
              quick_sort(a, left_index, pivot_index - 1)
            end

            # if right_index has not reached pivot, keep sorting
            if pivot_index < right_index
              quick_sort(a, pivot_index, right_index)
            end

            return a
          end
        ```


3. Heap Sort
    - It turns an array into a binary heap then repeatdely finds the largest element in the heap and orders it into the back of the array, building out the sorted list from back to front.
    - Big(O):
      - Time complexity: Building a max heap out of an array takes `O(n)`. Heapifying it takes `O(logn)`. So the toal time complexity is `O(nlogn)`.
      - Space complexity: It is in-place/internal and uses up little memory = `O(1)`.
    - Uses: Heap sort is popular for its efficiency and the fact that its time complexity is the same in all scenarios (unlike quick sort), so it is used in systems with critical response time e.g. linux kernel.
    - Instructions:
      1. Take an array and build it into a max heap i.e. root node has largest value.
      2. Heapify everything except the root node.
      3. Swap the root node with the last node in the heap. It's now sorted so we can remove it from the heap.
      4. The heap order prinicple has now been violated. Heapify the heap once more.
      5. Repeat steps 3-4 until the heap is empty and all items have been sorted.
    - Code:
      ```ruby
        def heap_sort(a)
          build_max_heap(a)

          # find last element
          last_index = a.length-1

          # continue heap sorting till 1 item is left
          while last_index > 0
            swap(a, 0, last_index)
            heapify(a)

            last_index -= 1
          end
        end

        def swap(array, first_index, last_index)
          array[first_index], array[last_index] = array[last_index], array[first_index]
          return array
        end
      ```
