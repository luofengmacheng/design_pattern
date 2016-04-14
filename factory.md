## 工厂模式

### 1 定义

> 工厂模式：定义了一个创建对象的接口，但由子类决定要实例化的类是哪一个，工厂模式让类把实例化推迟到子类。

### 2 C++代码

``` C++
// 需要实例化的类的抽象类
class Pizza {
public:
	virtual void box() = 0;
};

class NYStyleCheesePizza : public Pizza {
public:
	void box() {
		cout << "NYStyleCheesePizza box" << endl;
	}
};

class NYStyleVeggiePizza : public Pizza {
public:
	void box() {
		cout << "NYStyleVeggiePizza box" << endl;
	}
};

class ChicagoStyleCheesePizza : public Pizza {
public:
	void box() {
		cout << "ChicagoStyleCheesePizza box" << endl;
	}
};

class ChicagoStyleVeggiePizza : public Pizza {
public:
	void box() {
		cout << "ChicagoStyleVeggiePizza box" << endl;
	}
};

// 工厂的抽象类
class PizzaStore {
public:
	// 实例化对象的纯虚函数，当调用工厂实例的该函数时，
	// 会根据工厂的实际类型，调用对应的工厂函数
	virtual Pizza *createPizza(string) = 0;
	Pizza* orderPizza(string type) {
		Pizza *pizza = createPizza(type);

		pizza->box();

		return pizza;
	}
};

class NYPizzaStore : public PizzaStore {
	Pizza *createPizza(string type) {
		if(type == "cheese") {
			return new NYStyleCheesePizza;
		}
		else if(type == "veggie") {
			return new NYStyleVeggiePizza;
		}
	}
};

class ChicagoPizzaStore : public PizzaStore {
	Pizza *createPizza(string type) {
		if(type == "cheese") {
			return new ChicagoStyleCheesePizza;
		}
		else if(type == "veggie") {
			return new ChicagoStyleVeggiePizza;
		}
	}
};

int main(int argc, char const *argv[])
{
	PizzaStore *nypizzastore = new NYPizzaStore;
	nypizzastore->orderPizza("cheese");

	PizzaStore *chicagoPizzaStore = new ChicagoPizzaStore;
	chicagoPizzaStore->orderPizza("veggie");

	return 0;
}
```