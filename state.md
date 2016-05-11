## 状态模式

### 1 定义

> 状态模式：允许对象在内部状态改变时改变它的行为，对象看起来好像修改了它的类。

### 2 C++

``` C++
class Radio;

class State {
public:
	virtual void toggle() = 0;
	virtual void show() = 0;
};

class AmState : public State {
	Radio *radio;

public:
	AmState(Radio *radio) {
		this->radio = radio;
	}

	void toggle();
	void show();
};

class FmState : public State {
	Radio *radio;

public:
	FmState(Radio *radio) {
		this->radio = radio;
	}

	void toggle();
	void show();
};

class Radio {
	State *state;

public:
	AmState *amState;
	FmState *fmState;

	Radio() {
		amState = new AmState(this);
		fmState = new FmState(this);
		state = amState;
	}

	void toggle() {
		state->toggle();
	}

	void set_state(State *state) {
		this->state = state;
	}

	void get_current() {
		state->show();
	}
};

void AmState::toggle() {
	radio->set_state(radio->fmState);
}

void AmState::show() {
	cout << "AM state" << endl;
}

void FmState::toggle() {
	radio->set_state(radio->amState);
}

void FmState::show() {
	cout << "FM state" << endl;
}

int main(int argc, char const *argv[])
{
	Radio *radio = new Radio;
	radio->get_current();
	radio->toggle();
	radio->get_current();
	radio->toggle();
	radio->get_current();

	return 0;
}
```