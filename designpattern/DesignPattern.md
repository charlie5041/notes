---
title: CPP / C++ Notes - Design Patterns
tags: 
notebook: DesignPattern(C++)
---

<!-- TOC -->

- [General Techniques / Mechanism for Code Reuse](#general-techniques--mechanism-for-code-reuse)
  - [Inheritance](#inheritance)
    - [Dynamic / Runtime / Subtyping Polymorphism.](#dynamic--runtime--subtyping-polymorphism)
    - [Benefits:](#benefits)
    - [Drawbacks:](#drawbacks)
  - [Delegation (a kind of OO composition)](#delegation-a-kind-of-oo-composition)
    - [Use another object for implementing some functionality and forward a method call to it instead of inheriting the object class. Examples: store items in a std::vector, instead of storing them directly in the class with heap-allocated arrays; store an heap-allocated object using smart pointers rather than storing it using raw pointer and freeing it on the destructor.](#use-another-object-for-implementing-some-functionality-and-forward-a-method-call-to-it-instead-of-inheriting-the-object-class-examples-store-items-in-a-stdvector-instead-of-storing-them-directly-in-the-class-with-heap-allocated-arrays-store-an-heap-allocated-object-using-smart-pointers-rather-than-storing-it-using-raw-pointer-and-freeing-it-on-the-destructor)
  - [Static Polymorphism](#static-polymorphism)
    - [It is templates and overloaded functions or methods.](#it-is-templates-and-overloaded-functions-or-methods)
    - [Templates can eliminate virtual method call overhead by resolving methods at compile-time rather than at runtime.](#templates-can-eliminate-virtual-method-call-overhead-by-resolving-methods-at-compile-time-rather-than-at-runtime)
  - [Type Erasure](#type-erasure)
    - [Combination of static and dynamic polymorphism which gets the best of both sides with minimal overhead.](#combination-of-static-and-dynamic-polymorphism-which-gets-the-best-of-both-sides-with-minimal-overhead)
  - [Free Functions](#free-functions)
    - [C++ is a multiparadigm language and not everything needs to to be a class. So, functions can be used to extending classes without modifying them by taking objects as arguments and applying operations on them.](#c-is-a-multiparadigm-language-and-not-everything-needs-to-to-be-a-class-so-functions-can-be-used-to-extending-classes-without-modifying-them-by-taking-objects-as-arguments-and-applying-operations-on-them)
  - [Higher Order Functions / Higher Order Methods / Lambda Functions](#higher-order-functions--higher-order-methods--lambda-functions)
- [Singleton](#singleton)

<!-- /TOC -->

[reference](https://caiorss.github.io/C-Cpp-Notes/cpp-design-patterns.html)

# General Techniques / Mechanism for Code Reuse
## Inheritance
### Dynamic / Runtime / Subtyping Polymorphism.
### Benefits:
- Allow switching implementation at runtime, store different objects (pointers, smart pointers or references) at the same container, defer method calls to specific implementation and allow a client code work with multiple different implementations at runtime. Inheritance alone is not evil, the problem is deep inheritance hierarchies.
### Drawbacks:
- Loss of value semantics and intrusive.
- Classes must inherit from the same base class which requires modification.
- Objects can only be returned from factory functions as pointers or pointers wrapped by smart pointers.
- Slower speed than static polymorphism (templates and overloaded functions or methods.)
## Delegation (a kind of OO composition)
### Use another object for implementing some functionality and forward a method call to it instead of inheriting the object class. Examples: store items in a std::vector, instead of storing them directly in the class with heap-allocated arrays; store an heap-allocated object using smart pointers rather than storing it using raw pointer and freeing it on the destructor.
## Static Polymorphism
### It is templates and overloaded functions or methods.
### Templates can eliminate virtual method call overhead by resolving methods at compile-time rather than at runtime.
## Type Erasure
### Combination of static and dynamic polymorphism which gets the best of both sides with minimal overhead.
## Free Functions
### C++ is a multiparadigm language and not everything needs to to be a class. So, functions can be used to extending classes without modifying them by taking objects as arguments and applying operations on them.
## Higher Order Functions / Higher Order Methods / Lambda Functions
###　Lambda function can be used to add new behaviors to an object at runtime, create generalized algorithms, functions, simplify event handling, callbacks and design patterns.

# Singleton
Singleton is a creational design pattern where there is only a single instance of a class and client code is forbidden from creating new instances.

**Note: Despite that there are lots of objections against this design pattern, it is still worth knowing how it works.**

- File: file:src/design-patterns/singleton1.cpp
- Onlien Compiler: https://rextester.com/SJWB72590
Header of class FileRepository => (FileRepository.hpp)

```cpp
//----- file: FileRepository.hpp ------//
#ifndef __file_repository 
#define __file_repository
#include <std::string>

class FileRepository {
 private:
  std::deque<std::string> _files; 
  // Forbid client code instating a new instance. 
  FileRepository(){
    std::cerr << " [LOG] File Respository Initialized." << "\n";
  }   
  // Forbid client code from creating a copy or using the
  // copy constructor.
  FileRepository(const FileRepository&){}     
 public:
  ~FileRepository();

  // Return a reference to not allow client code 
  // to delete object.    
  static auto getInstance() -> FileRepository&;   
  // Use old C++ 'member function' syntax.
  void addFile(std::string fname);
  void clearFiles();
  // C++11 member function declaration looks better. 
  auto showFiles() -> void;
};
#endif
``` 
Implementation of FileRepository (FileRepository.cpp)

```cpp
#include <iostream>
#include "FileRepository.hpp"

FileRepository::~FileRepository(){
  std::cerr << " [LOG] File Respository Deleted. Ok." << "\n";
}

// Static method 
auto FileRepository::getInstance() -> FileRepository& {
  static auto instance = std::unique_ptr<FileRepository>{nullptr};       
  // Initialized once - lazy initialization 
  if(!instance)
    instance.reset(new FileRepository);       
  return *instance.get();
}

void FileRepository::addFile(std::string fname){
  _files.push_back(std::move(fname));
}

void FileRepository::clearFiles(){
  _files.clear();
}
// C++11 Member function declaration 
auto FileRepository::showFiles() -> void {
  for(const auto& file: _files)
    std::cout << " File = " << file << std::endl;
}
```
Main function:
```cpp
//---- It should be file: main.cpp -------------//
int main(){

  FileRepository& repo1 = FileRepository::getInstance();
	repo1.addFile("CashFlowStatement.txt");
	repo1.addFile("Balance-Sheet.dat");
	repo1.addFile("Sales-Report.csv");
	FileRepository& repo2 = FileRepository::getInstance();
	
	std::cout << std::boolalpha << "Same object? (&repo == &repo1 ?) = "
			  << (&repo1 == &repo2)
			  << "\n";
	std::cout << "Repository files" << std::endl;
	repo2.showFiles();

	std::cout << "Add more files" << std::endl;
	repo2.addFile("fileX1.pdf");
	repo2.addFile("fileX2.pdf");
	repo2.addFile("fileX3.pdf");
	repo2.showFiles();
	
	return EXIT_SUCCESS;
}