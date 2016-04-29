## 迭代器模式

### 1 定义

> 迭代器模式：提供一种方法顺序访问一个聚合对象中的各个元素，而又不暴露其内部的表示。

### 2 C++

``` C++
// 类似于STL实现的迭代器
template <class T>
class Vector {
private:
	T *data;

	int len;
public:
	Vector() {
		data = new T[10];
		len = 0;

		cout << "vector initialize" << endl;
	}

	~Vector() {
		delete []data;

		cout << "vector uninitialize" << endl;
	}

	// 由于迭代器知道关于对应类的一切
	// 因此，将迭代器类作为对应类的内部类
	class VectorIterator {
		T *ptr;

	public:
		VectorIterator(T *ptr) {
			this->ptr = ptr;
		}

		T operator++() {
			++ptr;
			return *ptr;
		}

		T operator*() {
			return *ptr;
		}

		bool operator !=(VectorIterator iter) {
			return this->ptr != iter.ptr;
		}
	};

	// 定义迭代器类型
	typedef VectorIterator iterator;

	iterator begin() {
		return iterator(data);
	}

	iterator end() {
		return iterator(data + len);
	}

	void push_back(T d) {
		data[len++] = d;
	}
};

// 以下实现类似于Java中的迭代器

// 迭代器基类
template <class T>
class Iterator {
public:
	virtual Iterator* next()  = 0;
	virtual bool hasNext() = 0;
	virtual T operator *() = 0;
};

// vector的迭代器类
template <class T>
class VecIterator : public Iterator<T> {
private:
	T *ptr;
	int len;
	int size;
	int cur;

public:
	VecIterator(T *ptr, int len, int size) {
		this->ptr = ptr;
		this->len = len;
		this->size = size;
		this->cur = 0;
	}

	Iterator<T>* next() {
		if(cur == len - 1) {
			return this;
		}
		else {
			++ptr;
			++cur;
			return this;
		}
	}

	T operator *() {
		return *ptr;
	}

	bool hasNext() {
		return cur < len;
	}
};

// vector类
template <class T>
class Vec {
private:
	T *data;
	int len;
	int size;

public:
	Vec() {
		data = new T[10];
		len = 0;
		size = 10;

		cout << "vec initialize" << endl;
	}

	~Vec() {
		delete []data;

		cout << "vec uninitialize" << endl;
	}

	// 创建迭代器
	Iterator<T>* iterator() {
		return new VecIterator<T>(data, len, size);
	}

	void push_back(T d) {
		data[len++] = d;
	}
};

int main(int argc, char const *argv[])
{
    // 类似于STL遍历的方式
	Vector<int> vec;
	vec.push_back(10);
	vec.push_back(20);
	for(Vector<int>::iterator iter = vec.begin(); iter != vec.end(); ++iter) {
		cout << *iter << endl;
	}

	// 类似于Java遍历的方式
	Vec<int> vv;
	vv.push_back(10);
	vv.push_back(20);
	Iterator<int> *iter = vv.iterator();
	while(iter->hasNext()) {
		cout << *(*iter) << endl;
		iter = iter->next();
	}

	return 0;
}
```

### 3 C++与Java的几点不同之处

从上面的实现，并对比Java本身的操作方式，可以知道C++与Java的几点不同：

* Java中所有的类型的基类都是Object，因此，在写泛型时，可以充分利用该特性，直接将返回类型设置为Object，就可以返回任意类型的对象。而C++中是没有这种基类。
* Java中的对象具有多态性，C++中的对象不具有多态性，传递指针或者引用才具有多态性。这就导致了，迭代器在返回类型时，只能返回指针类型，而不能直接返回对象。
* Java是纯面向对象的，它的类都有继承关系，C++是部分面向对象的，而且STL的实现中，也基本没有用到继承关系，各个类都是独立编写的。