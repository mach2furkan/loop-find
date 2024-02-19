     #include <iostream>
    #include <unordered_set>

      // Define LinkedListNode structure
     struct LinkedListNode {
    int value;
    LinkedListNode* next;
    LinkedListNode(int val) : value(val), next(nullptr) {}
     };

        // Function to find the starting node of the loop
      LinkedListNode* filter(LinkedListNode* L) {
    LinkedListNode* slow = L;
    LinkedListNode* fast = L;

    // Detect cycle
    while (fast != nullptr && fast->next != nullptr) {
        slow = slow->next;
        fast = fast->next->next;
        if (slow == fast) {
            break;
        }
    }

    // If no cycle found
    if (fast == nullptr || fast->next == nullptr) {
        return nullptr;
    }

    // Move one pointer to the head and advance both pointers at the same pace
    slow = L;
    while (slow != fast) {
        slow = slow->next;
        fast = fast->next;
    }

    // Both pointers now meet at the start of the cycle
    return slow;
    }

    int main() {
    // Example usage
    LinkedListNode* head = new LinkedListNode(1);
    head->next = new LinkedListNode(2);
    head->next->next = new LinkedListNode(3);
    head->next->next->next = new LinkedListNode(4);
    head->next->next->next->next = new LinkedListNode(5);
    head->next->next->next->next->next = head->next->next; // Creating the loop

    // Find the starting node of the loop
    LinkedListNode* start_of_loop = filter(head);
    if (start_of_loop != nullptr) {
        std::cout << "Start of the loop: " << start_of_loop->value << std::endl; // Output: 3
    } else {
        std::cout << "No loop found" << std::endl;
    }

    // Clean up memory
    LinkedListNode* curr = head;
    std::unordered_set<LinkedListNode*> visited;
    while (curr != nullptr) {
        if (visited.find(curr) != visited.end()) {
            break; // Loop detected, stop cleaning
        }
        visited.insert(curr);
        LinkedListNode* temp = curr;
        curr = curr->next;
        delete temp;
    }

    return 0;
     }
