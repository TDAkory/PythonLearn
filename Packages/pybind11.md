# [pybind11](https://pybind11.readthedocs.io/en/stable/index.html)

pybind11 is a lightweight header-only library that exposes C++ types in Python and vice versa, mainly to create Python bindings of existing C++ code.

## pybind11 can map the following core C++ features to Python:

* Functions accepting and returning custom data structures per value, reference, or pointer

* Instance methods and static methods

* Overloaded functions

* Instance attributes and static attributes

* Arbitrary exception types

* Enumerations

* Callbacks

* Iterators and ranges

* Custom operators

* Single and multiple inheritance

* STL data structures

* Smart pointers with reference counting like std::shared_ptr

* Internal references with correct reference counting

* C++ classes with virtual (and pure virtual) methods can be extended in Python

## Functions

### Return Value

python和C++管理内存和对象生命周期的手段不同。在为返回非平凡类型对象的函数创建绑定时，这可能会导致问题，因为不清楚对象的释放应该由那一边负责。为此，pybind11 提供了几个可以传递给 module_::def() 和 class_::def() 函数的返回值策略。

- **return_value_policy::take_ownership：** Reference an existing object (i.e. do not create a new copy) and take ownership. Python will call the destructor and delete operator when the object’s reference count reaches zero. Undefined behavior ensues when the C++ side does the same, or when the data was not dynamically allocated.
- **return_value_policy::copy：** Create a new copy of the returned object, which will be owned by Python. 
- **return_value_policy::move** Use std::move to move the return value contents into a new instance that will be owned by Python.
- **return_value_policy::reference：** Reference an existing object, but do not take ownership. The C++ side is responsible for managing the object’s lifetime and deallocating it when it is no longer used. Warning: undefined behavior will ensue when the C++ side deletes an object that is still referenced and used by Python.
- **return_value_policy::reference_internal：** Indicates that the lifetime of the return value is tied to the lifetime of a parent object, namely the implicit this, or self argument of the called method or property. Internally, this policy works just like return_value_policy::reference but additionally applies a keep_alive<0, 1> call policy (described in the next section) that prevents the parent object from being garbage collected as long as the return value is referenced by Python. This is the default policy for property getters created via def_property, def_readwrite, etc.
- **return_value_policy::automatic：** This policy falls back to the policy return_value_policy::take_ownership when the return value is a pointer. Otherwise, it uses return_value_policy::move or return_value_policy::copy for rvalue and lvalue references, respectively.
- **return_value_policy::automatic_reference：** As above, but use policy return_value_policy::reference when the return value is a pointer. This is the default conversion policy for function arguments when calling Python functions manually from C++ code (i.e. via handle::operator()) and the casters in pybind11/stl.h. You probably won’t need to use this explicitly.

### Call Policy

In addition to the above return value policies, further call policies can be specified to indicate dependencies between parameters or ensure a certain state for the function call.

#### [Keep alive](https://pybind11.readthedocs.io/en/latest/advanced/functions.html#keep-alive)

In general, this policy is required when the C++ object is any kind of container and another object is being added to the container. keep_alive<Nurse, Patient> indicates that the argument with index Patient should be kept alive at least until the argument with index Nurse is freed by the garbage collector.

Argument indices start at one, while zero refers to the return value. For methods, index 1 refers to the implicit this pointer, while regular arguments begin at index 2.

#### [Call Guard](https://pybind11.readthedocs.io/en/latest/advanced/functions.html#call-guard)

The call_guard<T> policy allows any scope guard type T to be placed around the function call. 

#### Python objects as arguments

#### Accepting *args and **kwargs

#### Default arguments revisited
