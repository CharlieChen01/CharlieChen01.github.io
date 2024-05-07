+++
title = 'Smart Pointers in C++'
date = 2024-05-07T10:12:55+08:00
tags = ['C++', 'Smart Pointers']
draft = false
+++

Let’s explore some examples of using std::unique_ptr, std::shared_ptr, and std::weak_ptr in C++.

- ## std::unique_ptr:

  std::unique_ptr is designed for exclusive ownership of a dynamically allocated object. It ensures that there can be at most one unique_ptr pointing to any resource.
  When the unique_ptr is destroyed, the resource it points to is automatically reclaimed.
  You cannot make a copy of a unique_ptr, but you can move it using the new move semantics.
  Example:

```c++
#include <iostream>
#include <memory>

int main() {
    std::unique_ptr<int> uptr(new int(42));
    std::cout << "Value: " << *uptr << std::endl;

    // Moving ownership to another unique_ptr
    std::unique_ptr<int> uptr2 = std::move(uptr);
    std::cout << "Value (moved): " << *uptr2 << std::endl;

    // uptr is now empty

    return 0;
}
```

In this example, uptr2 takes ownership of the dynamically allocated integer, and uptr becomes empty after the move.

- ## std::shared_ptr:

  std::shared_ptr allows multiple pointers to share ownership of the same resource.
  When the last shared_ptr pointing to a resource is destroyed, the resource is deallocated.
  Example:

```c++
#include <iostream>
#include <memory>

int main() {
    std::shared_ptr<int> sptr1 = std::make_shared<int>(10);
    std::shared_ptr<int> sptr2 = sptr1; // Both share ownership

    std::cout << "Value (sptr1): " << *sptr1 << std::endl;
    std::cout << "Value (sptr2): " << *sptr2 << std::endl;

    // When both sptr1 and sptr2 go out of scope, the resource is deallocated

    return 0;
}
```

In this example, both sptr1 and sptr2 share ownership of the integer value 10.

- ## std::weak_ptr:

  std::weak_ptr represents a weak form of shared ownership.
  It doesn’t affect the reference count of the resource.
  You can convert a weak_ptr to a shared_ptr on-demand if the resource still exists.
  Example:

```c++
#include <iostream>
#include <memory>

int main() {
    std::shared_ptr<int> sharedPtr = std::make_shared<int>(20);
    std::weak_ptr<int> weakPtr = sharedPtr;

    // Access the resource through weakPtr (if it still exists)
    if (auto lockedPtr = weakPtr.lock()) {
        std::cout << "Value (weakPtr): " << *lockedPtr << std::endl;
    } else {
        std::cout << "Resource no longer exists." << std::endl;
    }

    // When sharedPtr goes out of scope, the resource is deallocated

    return 0;
}
```

In this example, weakPtr allows access to the resource if it still exists, but it doesn’t prevent deallocation.
Remember to choose the appropriate smart pointer based on your ownership requirements and resource-sharing needs!
