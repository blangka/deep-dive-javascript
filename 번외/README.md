## 그 밖에 번외편
### generic
generic을 사용하는 이유는 다음과 같다 아래 예시를 참고하자
~~~javascript
function makeNumberArray(defaultValue: number, size: number): number[] {
  const arr: number[] = [];
  for (let i = 0; i < size; i++) {
    arr.push(defaultValue);
  }
  return arr;
}
makeNumberArray(1, 10); // [1, 1, 1, 1, 1, 1, 1, 1, 1, 1]
~~~
위 함수는 number 타입의 배열을 만드는 함수이다.
~~~javascript
function makeStringArray(defaultValue: string, size: number): string[] {
  const arr: string[] = [];
  for (let i = 0; i < size; i++) {
    arr.push(defaultValue);
  }
  return arr;
}
~~~
위 함수는 string 타입의 배열을 만드는 함수이다.
위 두 함수는 기능은 같지만 타입만 다르다. 이런 경우에 generic을 사용하면 다음과 같이 사용할 수 있다.
~~~javascript
function makeArray<T>(defaultValue: T, size: number): T[] {
  const arr: T[] = [];
  for (let i = 0; i < size; i++) {
    arr.push(defaultValue);
  }
  return arr;
}
~~~
generic을 funtion이 아닌 const로 사용할 수도 있다.
~~~typescript
const makeArray = <T>(defaultValue: T, size: number): T[] => {
  const arr: T[] = [];
  for (let i = 0; i < size; i++) {
    arr.push(defaultValue);
  }
  return arr;
};
~~~


아래와 같이 첫번째 요소로 들어가는 type을 알수 없는 경우 일반적으로 이렇게 짠 경험이 있을 것이다.
~~~javascript
function firstElement(arr: any[]) {
  return arr[0];
}
~~~
이런 경우에도 generic을 사용하면 다음과 같이 사용할 수 있다.
~~~javascript
function firstElement<T>(arr: T[]): T {
  return arr[0];
}
~~~

일예로 만들었던 만들었던 customHook에서는 다음과 같이 사용하였다.

~~~typescript
/*
 * @param initialStep: string - 초기 step
 * @desc step 별로 노출을 관리하는 hook
 */
const useFunnel = <TFunnelSteps,>(initialStep: TFunnelSteps) => {
  const [step, setStep] = useState(initialStep);

  const Funnel = (props: IFunnelProps<TFunnelSteps>) => {
    const { children, name } = props;
    return <>{step === name && children}</>;
  };

  const currentStep = () => {
    return step;
  };

  const changeStep = (step: TFunnelSteps) => {
    setStep(step);
  };

  return { Funnel, currentStep, changeStep };
};

export default useFunnel;
~~~
