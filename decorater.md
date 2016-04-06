## 装饰者模式

### 1 定义

> 装饰者模式：动态地将责任附加到对象上。若要扩展功能，装饰者提供了比继承更有弹性的替代方案。

### 2 C++代码

``` C++

// 装饰者和被装饰者的基类
class Beverage {
protected:
	string description;

public:
	Beverage() : description("unknown Beverage") { }

	virtual string getDescription() {
		return description;
	}

	virtual double cost()  = 0;
};

// 被装饰者1
class Espresso : public Beverage {
public:
	Espresso() {
		description = "Espresso";
	}

	double cost() {
		return 1.99;
	}
};

// 被装饰者2
class HouseBlend : public Beverage {
public:
	HouseBlend() {
		description = "House Blend Coffee";
	}

	double cost() {
		return 0.89;
	}
};

// 装饰者1
class Mocha : public Beverage {
private:
	// 被装饰者1装饰的对象
	Beverage *beverage;

public:
	Mocha(Beverage *beverage) {
		this->beverage = beverage;
	}

	string getDescription() {
		return beverage->getDescription() + ", Mocha";
	}

	double cost() {
		return 0.20 + beverage->cost();
	}
};

// 装饰者2
class Whip : public Beverage {
private:
	Beverage *beverage;

public:
	Whip(Beverage *beverage) {
		this->beverage = beverage;
	}

	string getDescription() {
		return beverage->getDescription() + ", Mocha";
	}

	double cost() {
		return 0.10 + beverage->cost();
	}
};

int main(int argc, char const *argv[])
{
	Beverage *beverage = new Espresso();

	cout << beverage->getDescription() << " $" << beverage->cost() << endl;

	Beverage *beverage2 = new HouseBlend();

	beverage2 = new Mocha(beverage2);
	beverage2 = new Mocha(beverage2);
	beverage2 = new Whip(beverage2);

	cout << beverage2->getDescription() << " $" << beverage2->cost() << endl;

	return 0;
}

```