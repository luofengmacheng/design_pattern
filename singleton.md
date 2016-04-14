## 单例模式

### 1 定义

> 单例模式：确保一个类只有一个实例，并提供一个全局访问点。

### 2 C++

下面的代码在遭遇多线程时，会有问题，可以参照：[实现Singleton模式](https://github.com/luofengmacheng/algorithms/blob/master/interviewOffer/2.md)

``` C++
/**
 * 单例模式
 */
class Singleton {

private:
	/**
	 * 要点1 将构造函数设置为私有函数
	 */
	Singleton() { }

	/**
	 * 要点2 保存单例对象的指针
	 */
	static Singleton *instance;

public:

	/**
	 * 要点3 获取单一实例的函数
	 * @return 单一实例
	 */
	static Singleton *GetInstance() {
		if(instance == NULL) {

			/**
			 * 如果还没有生成实例，则new一个实例
			 */
			instance = new Singleton;
		}
		return instance;
	}

	void func() {
		cout << "singleton func" << endl;
	}
};

/**
 * 类的静态成员初始化
 */
Singleton *Singleton::instance = NULL;

int main(int argc, char const *argv[])
{
	Singleton *singleton = Singleton::GetInstance();
	singleton->func();

	return 0;
}

```