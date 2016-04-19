## 命令模式

### 1 定义

> 命令模式：将请求封装成对象，这可以让你使用不同的请求、队列，或者日志请求来参数话其它对象。

### 2 C++

``` C++
/**
 * 命令对象的接口
 */
class Command {
public:
	virtual void execute() = 0;
};

/**
 * 实际操作的对象
 */
class Light {
public:
	void on() {
		cout << "turn on light" << endl;
	}
};

/**
 * 将实际操作封装成命令对象
 */
class LightOnCommand : public Command {
	Light *light;

public:
	LightOnCommand(Light *light) {
		this->light = light;
	}

	void execute() {
		light->on();
	}
};

/**
 * 发出请求的对象
 */
class SimpleRemoteControl {
	Command *slot;

public:
	void setCommand(Command *command) {
		slot = command;
	}

	void buttonWasPressed() {
		slot->execute();
	}
};

int main(int argc, char const *argv[])
{
	SimpleRemoteControl *control = new SimpleRemoteControl;
	Light *light = new Light;
	LightOnCommand *lightOn = new LightOnCommand(light);

	control->setCommand(lightOn);
	control->buttonWasPressed();

	return 0;
}
```