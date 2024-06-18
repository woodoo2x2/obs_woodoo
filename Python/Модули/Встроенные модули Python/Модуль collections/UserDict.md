Исходный код класса `UserDict`:
```python
class UserDict(_collections_abc.MutableMapping):  
  
    # Start by filling-out the abstract methods  
    def __init__(self, dict=None, /, **kwargs):  
        self.data = {}  
        if dict is not None:  
            self.update(dict)  
        if kwargs:  
            self.update(kwargs)  
  
    def __len__(self):  
        return len(self.data)  
  
    def __getitem__(self, key):  
        if key in self.data:  
            return self.data[key]  
        if hasattr(self.__class__, "__missing__"):  
            return self.__class__.__missing__(self, key)  
        raise KeyError(key)  
  
    def __setitem__(self, key, item):  
        self.data[key] = item  
  
    def __delitem__(self, key):  
        del self.data[key]  
  
    def __iter__(self):  
        return iter(self.data)  
  
    # Modify __contains__ and get() to work like dict  
    # does when __missing__ is present.    def __contains__(self, key):  
        return key in self.data  
  
    def get(self, key, default=None):  
        if key in self:  
            return self[key]  
        return default  
  
  
    # Now, add the methods in dicts but not in MutableMapping  
    def __repr__(self):  
        return repr(self.data)  
  
    def __or__(self, other):  
        if isinstance(other, UserDict):  
            return self.__class__(self.data | other.data)  
        if isinstance(other, dict):  
            return self.__class__(self.data | other)  
        return NotImplemented  
  
    def __ror__(self, other):  
        if isinstance(other, UserDict):  
            return self.__class__(other.data | self.data)  
        if isinstance(other, dict):  
            return self.__class__(other | self.data)  
        return NotImplemented  
  
    def __ior__(self, other):  
        if isinstance(other, UserDict):  
            self.data |= other.data  
        else:  
            self.data |= other  
        return self  
  
    def __copy__(self):  
        inst = self.__class__.__new__(self.__class__)  
        inst.__dict__.update(self.__dict__)  
        # Create a copy and avoid triggering descriptors  
        inst.__dict__["data"] = self.__dict__["data"].copy()  
        return inst  
  
    def copy(self):  
        if self.__class__ is UserDict:  
            return UserDict(self.data.copy())  
        import copy  
        data = self.data  
        try:  
            self.data = {}  
            c = copy.copy(self)  
        finally:  
            self.data = data  
        c.update(self)  
        return c  
  
    @classmethod  
    def fromkeys(cls, iterable, value=None):  
        d = cls()  
        for key in iterable:  
            d[key] = value  
        return d
```