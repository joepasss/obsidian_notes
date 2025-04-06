### ADC
Analog Digital Convert

### DAC
Digital Analog Convert

 ``` mermaid
 graph LR
	 Analog -- ADC --> Digital
	 Digital -- DAC --> Analog
```

### Quntization

-1.0v, 1.0v 사이의 신호를 가지는 아날로그 신호를 8비트로 양자화 한다고 가정하면, -1.0v는 0의 값을 가지고, 1.0v 는 255의 값을 가짐 

ADC 과정에서, 양자화를 진행해야 하는데, 양자화 진행할 때 각 샘플 하나당 +- 1/2 LSB 에 해당하는 에러를 가질 수 있음 대부분의 경우에 양자화 에러는 신호에 정해진 만큼의 무작위 noise를 추가함 이 노이즈는 +- 1/2 LSB 사이의 균일분포를 가짐 평균은 0, 표준편차는 약 0.29 LSB

8비트일 때, `0.29/256` 약 1/900 정도의 노이즈
12비트일 때, `0.29/4096` 약 1/14000 정도의 노이즈
16비트일 때, `0.29/65546` 약 1/227000 정도의 노이즈

### Dithering

2/3 LSB의 값을 가지는 노이즈를 추가해서 조금 더 정확한 value 를 얻을 수 있게 함

인풋 signal이 const 하게 3.0001 볼트의 출력을 가진다고 가정해 보면, ditering 없이 10000개의 샘플을 추출해서 값을 보면 다 3000의 값을 가질 것, ditering 으로 노이즈를 추가하게 되면 약 90퍼센트의 샘플은 3000의 값을, 나머지는 3001의 값을 가지게 되고 이를 평균내면 3000.1 의 값으로 quntization 하기 전 값에 더 근접한 결과를 얻을 수 있음

**subtractive dither**
* generate random number
* passing through DAC (produce add noise)
* digitiaztion
* subtract random number from digital signal using floating point arithmetic

가장 간단한 방법이지만, 항상 가능하지는 않음

**Sampling theorem(Niquist sampling theorem)**
continuous signal can be properly sampled, only if it does not contain frequency components above one-half of the sampling rate.

2000 samples/seconds를 가지는 Sampling rate는 아날로그 시그널이 composed of frequencies below 1000 cycles/second

analog signal 이 3Khz 의 범위를 가진다고 가정하면, 샘플링을 하기 위해서는 최소 6000sample/sec (6kHz) 또는 그 이상의 sample rate 가 필요함







