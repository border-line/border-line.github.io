---
layout: single
title : "[프로그래머스] 다리를 지나는 트럭 - 성능개선"
category: "algo"

---

문제 설명 : https://programmers.co.kr/learn/courses/30/lessons/42583

<u>쉽게 생각되는 풀이</u>와 <u>조금 복잡하지만 성능을 좋은 풀이</u>를 소개한다. 

Clean Code라는 책을 감동 받으며 읽었는데 <u>조금 복잡하지만 성능이 좋은 풀이</u>를 작성할 때는 Clean Code 책에서 소개한 원칙인 의미가 명확한 이름, 길이가 짧은 함수, 함수 내 동일한 추상화 수준을 지키려고 노력을 했다.

이 문제는 아래 5가지 단계로 풀 수 있으며 하나의 함수 안에서 하나의 while문을 통해 비교적 쉽게 풀 수 있다.

1. bridge라는 queue 선언
2. bridge에 트럭을 순차적으로 추가
3. 새로운 트럭 넣을 수 없는 경우 더미 트럭(무게가 0인 트럭) 추가
4. bridge length를 넘는 경우 맨 앞의 트럭을 bridge queue에서 제거
5. 2~4번 반복하며 시간을 계산하다가 마지막 트럭을 다리에 넣은 후 현재까지 계산된 시간에 다리 길이만큼의 초(마지막 트럭이 다리를 건너는 시간)를 추가

여기에서 성능에 영향을 주는 부분은 3번 부분이다. 모든 트럭은 어차피 한번씩 다리에 들어가야 하므로 다른 부분에는 개선의 여지가 많이 없다. 하지만 <u>더미 트럭은 한번에 1만큼 길이의 트럭을 넣느냐 필요한 만큼의 길이의 트럭을 넣느냐에 따라 성능이 달라질 수 있다.</u> 예를 들어 맨 앞의 트럭이 다리를 나가는데 필요한 길이가 4일 경우 길이가 1인 4개의 더미 트럭을 넣는냐, 길이가 4인 1개의 더미 트럭을 넣느냐에 따라 성능이 달라진다.

그래서 원래 풀었던 풀이에서 <u>차 사이의 간격(다리 위의 더미 트럭 길이)을 뜻하는 변수</u>를 주고 단순 queue의 사이즈가 아니라 <u>해당 변수의 크기까지 함께 참조해 4번 로직(다리 길이에 따라 트럭을 queue에서 제거)을 수행</u>하도록 했고 성능이 크게 개선된 것을 확인할 수 있었다.

## 성능 차이

### input

```java
int bride_length = 9162;
int weight = 8446;
int[] trucks = new int[]{8446, 7273, 2945, 5143, 7780, 8186, 3684, 5483, 325, 1862, 5292, 819, 197, 7205, 1476, 7540, 317, 6314, 2099, 1374, 1627, 109, 7207, 8234, 2257, 5617, 3810, 5747, 180, 658, 1024, 3162, 5639, 802, 7246, 3016, 4886, 3698, 1139, 7292, 877, 2684, 1519, 6277, 3842, 8160, 4188, 1861, 8193, 1121, 357, 2847, 6060, 3989, 1249, 1667, 2674, 1156, 5598, 5621, 506, 6958, 672, 6292, 7658, 8354, 2981, 3937, 5764, 7868, 7854, 6309, 7326, 8239, 554, 2970, 5821, 309, 7141, 6523, 7097, 3734, 3889, 4950, 296, 8391, 1572, 5021, 6723, 7707, 1789, 7176, 2604, 4122, 7828, 4092, 7775, 8071, 4474, 1042, 7481, 3244, 7210, 4485, 1973, 1009, 988, 595, 2054, 5386, 7613, 4151, 7181, 4229};
```

위 input에 대해 1000 번 반복시 소요 시간

- 기존 풀이(항상 길이가 1인 더미 트럭 추가)  : 7845ms

- 새로운 풀이(상황에 맞는 길이의 더미 트럭 추가) : 15ms

input에 따라 다르지만 500배 정도의 성능 차이가 나는 경우도 확인 된다.

## 코드 

### 성능 테스트 코드

```java
package com.company;

public class Main {

    public static void main(String[] args) {
		Solution solution = new Solution();
		Solution2 solution2 = new Solution2();

//		int bride_length = (int)(Math.random()*10000);
//		int weight = (int)(Math.random()*10000);
//		int truck_length = (int)(Math.random()*10000);
//		int[] trucks = new int[truck_length];
//		for(int i = 0; i < truck_length; i++){
//			int truckWeight = (int)(Math.random()*weight);
//			if(truckWeight <= 0){
//				trucks[i] = 1;
//			} else {
//				trucks[i] = truckWeight;
//			}
//		}

		int bride_length = 9162;
		int weight = 8446;
		int[] trucks = new int[]{8446, 7273, 2945, 5143, 7780, 8186, 3684, 5483, 325, 1862, 5292, 819, 197, 7205, 1476, 7540, 317, 6314, 2099, 1374, 1627, 109, 7207, 8234, 2257, 5617, 3810, 5747, 180, 658, 1024, 3162, 5639, 802, 7246, 3016, 4886, 3698, 1139, 7292, 877, 2684, 1519, 6277, 3842, 8160, 4188, 1861, 8193, 1121, 357, 2847, 6060, 3989, 1249, 1667, 2674, 1156, 5598, 5621, 506, 6958, 672, 6292, 7658, 8354, 2981, 3937, 5764, 7868, 7854, 6309, 7326, 8239, 554, 2970, 5821, 309, 7141, 6523, 7097, 3734, 3889, 4950, 296, 8391, 1572, 5021, 6723, 7707, 1789, 7176, 2604, 4122, 7828, 4092, 7775, 8071, 4474, 1042, 7481, 3244, 7210, 4485, 1973, 1009, 988, 595, 2054, 5386, 7613, 4151, 7181, 4229};

		long start = System.currentTimeMillis();
		for(int i = 0; i < 1000; i++){
			testSolution(bride_length, weight, trucks);
		}
		long end = System.currentTimeMillis();
		System.out.println("1 - " + (end - start));

		long start2 = System.currentTimeMillis();
		for(int i = 0; i<1000; i++) {
			testSolution2(bride_length, weight, trucks);
		}
		long end2 = System.currentTimeMillis();
		System.out.println("2 - " + (end2 - start2));
	}

	private static void testSolution(int bride_length, int weight, int[] trucks) {
		Solution solution = new Solution();
		solution.solution(bride_length, weight, trucks);
	}

	private static void testSolution2(int bride_length, int weight, int[] trucks) {
		Solution2 solution2 = new Solution2();
		solution2.solution(bride_length, weight, trucks);
	}
}

```



### 기존 풀이(항상 길이가 1인 더미 트럭 추가)

```java
import java.util.*;

class Solution {
	public int solution(int bridge_length, int weight, int[] truck_weights) {
		int answer = 0;
		int weightInBridge = 0;

		Queue<Integer> bridge = new LinkedList<>();

		for(int i = 0; i < truck_weights.length; i++){
			int currentTruckWeight = truck_weights[i];
			
			// 새로운 트럭을 다리에 넣을 수 없는 경우 맨 앞의 트럭 제거 및 더미 트럭 추가 
			while(weightInBridge + currentTruckWeight > weight){
				// 부등호가 >가 아니라 >=인 것은 요구사항이 맨 앞의 트럭이 나감과 동시에 새로운 트럭이 들어오는 것으로 가정하기 때문이다.
				// > 로 하는 경우 새로운 트럭을 추가할 때 맨 앞의 트럭이 빠지는지 여부에 따라 -1 초를 해주어야 한다. 
				if( bridge.size() >= bridge_length){
					weightInBridge -= bridge.poll();
				} else {
					bridge.offer(0);
					answer++;
				}
			}
			
			// 새로운 트럭을 다리에 추가
			bridge.offer(currentTruckWeight);
			weightInBridge += currentTruckWeight;
			answer++;
		}

    // 마지막 트럭이 다리를 지나는 것을 답에 포함시키기 위해 다리 길이만큼의 값 추가
		return answer + bridge_length;
	}
}
```



### 새로운 풀이(상황에 맞는 길이의 더미 트럭 추가)

```java
import java.util.*;

class Solution2 {
	private Queue<TruckInBridge> bridge = new LinkedList<>();

	private int requiredSeconds = 0;
	private int emptyDistanceBetweenCar = 0;
	private int weightInBridge = 0;
	private int truckInBridgeCnt = 0;

	private int weight = 0;
	private int bridge_length = 0;

	public int solution(int bridge_length, int weight, int[] truck_weights) {
		setBridgeLengthAndWeight(bridge_length, weight);
		calculateRequiredSecondsOfAllTrucks(truck_weights);
		return requiredSeconds;
	}

	private void setBridgeLengthAndWeight(int bridge_length, int weight){
		this.bridge_length = bridge_length;
		this.weight = weight;
	}

	private void calculateRequiredSecondsOfAllTrucks(int[] truck_weights) {
		addAllTrucksToTheBridge(truck_weights);
		waitUntilLastTruckGoThroughTheBridge();
	}

	private void addAllTrucksToTheBridge(int[] truck_weights) {
		for(int i = 0; i < truck_weights.length; i++){
			addTruckToBridgeByWaitingInCaseOfWeightFull(truck_weights[i]);
		}
	}

	private void addTruckToBridgeByWaitingInCaseOfWeightFull(int truck_weight) {
		while(isCannotEnterDueToWeight(truck_weight)){
			if(isCanExtractTruck()){
				extractOneTruckOfBridge();
			} else {
				addDummyTruckToBridge();
			}
		}

		addTruckToBridge(truck_weight);
	}

	private boolean isCannotEnterDueToWeight(int truckWeight){
		return weightInBridge + truckWeight > weight;
	}

	private boolean isCanExtractTruck() {
		return truckInBridgeCnt + emptyDistanceBetweenCar >= bridge_length;
	}

	private void extractOneTruckOfBridge() {
		TruckInBridge truck = this.bridge.remove();

		weightInBridge -= truck.weight;

		if(isDummyTruck(truck)){
			emptyDistanceBetweenCar -= (truck.length);
		} else {
			truckInBridgeCnt--;
		}
	}

	private boolean isDummyTruck(TruckInBridge frontTruckInBridge) {
		return frontTruckInBridge.weight == 0;
	}

	private void addDummyTruckToBridge() {
		TruckInBridge dummyTruck = makeDummyTruckOfRequiredLength();
		addDummyTruckToBridge(dummyTruck);
	}

	private TruckInBridge makeDummyTruckOfRequiredLength() {
		int requiredLength = getLengthBetweenFrontTruckAndEnd();

		TruckInBridge dummyTruck = new TruckInBridge(0);
		dummyTruck.length = requiredLength;

		return dummyTruck;
	}

	private int getLengthBetweenFrontTruckAndEnd() {
		return bridge_length - truckInBridgeCnt - emptyDistanceBetweenCar;
	}

	private void addDummyTruckToBridge(TruckInBridge dummyTruck) {
		bridge.add(dummyTruck);

		emptyDistanceBetweenCar += dummyTruck.length;
		requiredSeconds += dummyTruck.length;
	}

	private void addTruckToBridge(int truckWeight) {
		TruckInBridge truck = new TruckInBridge(truckWeight);
		bridge.add(truck);
		truckInBridgeCnt++;
		weightInBridge += truckWeight;
		requiredSeconds++;
	}

	private void waitUntilLastTruckGoThroughTheBridge() {
		requiredSeconds += bridge_length;
		bridge = new LinkedList<>();
		emptyDistanceBetweenCar = 0;
		truckInBridgeCnt = 0;
		weightInBridge = 0;
	}

	class TruckInBridge{
		public int length;
		public int weight;
		TruckInBridge(int weight){
			this.length = 1;
			this.weight = weight;
		}
	}
}
```

