## 模版方法模式

### 1 定义

> 模版方法模式：在一个方法中定义一个算法的骨架，而将一些步骤延迟到子类中。模版方法使得子类可以在不改变算法结构的情况下，重新定义算法中的某些步骤。

### 2 C++

``` C++
class Tool {
public:
	virtual void step_2 () = 0;

	void do_something() {
		step_1();
		step_2();
		step_3();
		hook();
	}

private:
	void step_1() {
		cout << "step 1" << endl;
	}

	void step_3() {
		cout << "step 3" << endl;
	}

	void hook() {}
};

class Concreate : public Tool {
public:
	void step_2() {
		cout << "Concreate step 2" << endl;
	}
};

int main(int argc, char const *argv[])
{
	Concreate *my_object = new Concreate;
	my_object->do_something();

	return 0;
}

```