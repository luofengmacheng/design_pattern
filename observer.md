## 观察者模式

### 1 定义

> 观察者模式：定义对象之间的一对多依赖，这样一来，当一个对象改变状态时，它的所有依赖者都会收到通知并自动更新。

这里涉及到两个对象：

可观察者：可观察者可以注册和删除观察者对象，只有经过注册的观察者才会收到状态更新通知。因此，它内部会保存一个观察者的队列。

观察者：当可观察者检测到状态变化时，会调用观察者的更新函数(假定是update)，更新函数可以获得可观察者的数据，因此，观察者的更新函数会使用可观察者的引用作为参数。

### 2 C++代码

``` C++
// 可观察者的前向声明
class Subject;

// 观察者的接口
class Observer {
public:
	// 更新函数
	// 当要创建一个新的观察者时，就要继承它
    virtual void update(Subject *) = 0;
};

// 可观察者基类
class Subject {
public:
	Subject() : observers() {
	}

	// 注册函数，将参数表示的观察者加入到观察者队列中
	void attach(Observer *observer) {
		observers.push_back(observer);
	}

	// 注销函数，将参数表示的观察者从观察者队列中删除
	void detach(Observer *observer) {
		for(vector<Observer *>::iterator iter = observers.begin(); iter != observers.end(); ++iter) {
			if((*iter) == observer) {
				observers.erase(iter);
				return;
			}
		}
	}

	// 通知函数，当状态改变时，调用此函数
	void notify() {
		for(vector<Observer *>::iterator iter = observers.begin(); iter != observers.end(); ++iter) {
			(*iter)->update(this);
		}
	}

	// 可观察者的数据返回函数
	virtual int get() {
	}

private:
	// 可观察者保存的观察者队列
	vector<Observer *> observers;
};

// 可观察者
class Data : public Subject {
public:
	Data(int data) : Subject() {
		this->data = data;
	}

	// 可观察者改变数据的状态，并通知观察者
	void set(int data) {
		this->data = data;
		Subject::notify();
	}

	int get() {
		return this->data;
	}

private:

	// 可观察者保存的数据
	int data;
};

// 实际的观察者
class ColorDisplay : public Observer {
public:
	ColorDisplay(string display_name) : Observer() {
		name = display_name;
	}

	// 它的唯一的特点是，定义了update函数，这也是可观察者唯一了解观察者的地方
	// 它的参数是可观察者的指针，用于获取可观察者的数据
	void update(Subject *subject) {
		cout << name << " data:" << subject->get() << endl;
	}

private:
	string name;
};

int main() {

	// 创建一个可观察者
	Data data1(1);

	// 创建两个观察者
	ColorDisplay cd1("red");
	ColorDisplay cd2("green");

	// 注册两个观察者
	data1.attach(&cd1);
	data1.attach(&cd2);
	
	// 改变数据的状态
	data1.set(1);
	data1.set(5);

	// 注销其中一个观察者
	data1.detach(&cd1);

	// 改变数据的状态，
	// 此时，第一个观察者不会收到通知
	data1.set(2);
	data1.set(3);
}
```

通过以上代码，重新又温习了C++的部分知识：

* 当类A定义在类B的后面，而类B使用了类A时，必须在类B前面前向声明类A，并且，类B只能使用类A的引用或者指针。
* 引用或者指针调用虚函数时，才会具有多态性，对象不具有多态性。
* 基类中有一个纯虚函数时，该类就成为一个虚基类，那么，不能定义它的对象。
* 类的成员有三种访问限制：public、protected、private，并且，默认为private。同样，也有三种类型的继承，默认为private继承，但是，通常会使用public继承，其它两种类型的继承很少使用。
* 类成员的初始化使用初始化列表形式，放在函数名的括号后面，基类的初始化也放在此处。

### 3 使用场景

观察者模式常用于GUI的设计中：当用户在界面上进行操作时，需要将操作的对象以及操作的状态传递给关心用户操作的对象，用户操作的对象就是`可观察者`，关系用户操作的对象就是`观察者`。