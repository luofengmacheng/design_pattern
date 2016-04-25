## 适配器模式

### 1 定义

> 适配器模式：将一个类的接口，转换成客户期望的另一个接口。适配器让原本接口不兼容的类可以合作无间。

### 2 C++

``` C++
/**
 * 客户端使用的接口
 */
class Target {
public:
	virtual void request() = 0;
};

/**
 * 具有客户使用接口的类
 */
class real_target : public Target {
public:
	void request() {
		cout << "real target request" << endl;
	}
};

/**
 * 客户端实际想要使用的类，该类没有request方法
 */
class Adaptee {
public:
	void do_something() {
		cout << "Adaptee do something" << endl;
	}
};

/**
 * 适配器：
 * 1 继承接口，有request方法
 * 2 方法委托，将request方法委托给实际操作对象的其它方法
 */
class Adapter : public Target {
	Adaptee *adaptee;

public:
	Adapter(Adaptee *adaptee) {
		this->adaptee = adaptee;
	}

	void request() {
		adaptee->do_something();
	}
};

void client(Target *target) {
	target->request();
}

int main(int argc, char const *argv[])
{
	Adaptee *adaptee = new Adaptee;
	Adapter *adapter = new Adapter(adaptee);
	client(adapter);

	return 0;
}
```