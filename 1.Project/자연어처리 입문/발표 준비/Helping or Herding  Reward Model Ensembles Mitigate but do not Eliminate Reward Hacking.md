![[Helping or Herding Reward Model Ensembles Mitigate but do not Eliminate Reward Hacking.pdf]]

## 1. 시작 내용

여러 text들로 학습한 llm = pretrained model
pretrained model + reward head + 인간 선호 데이터 fine-tuning = reward model
이때 첫 단계는 pretraining step, 다음 단계는 finetuning step이다.
각 스텝에서 random seed를 5개씩 두어서 총 25개의 reward model을 만든다

## 2. 앙상블 종류

- pretraining ensemble은 서로 다른 pretraining model을 같은 finetuning seed를 이용해서 만든 reward model들의 평균을 이용해서 reward를 구하는 방법

- finetune ensemble은 같은 pretraining model을 서로 다른 finetuning seed를 이용해서 만든 reward model들의 평균을 이용해서 reward를 구하는 방법

## 3. Ensemble 결과

실제로 더 나은 결과를 보여주었다.
특히나 Best-of-N 환경에서 pretrain ensemble이 가장 성능이 좋았다.

## 3-1. 핵심 원리와 Pessimism

- RM들은 학습 분포(ID)에서는 비슷했지만, 분포 밖(OOD) 답변에서는 판단이 달라지는 underspecification을 보였다.

- RLHF와 Best-of-N이 강해질수록 policy 출력이 RM의 학습 분포에서 멀어지고 RM 간 disagreement가 증가했다.

- 논문은 RM 점수를 Mean, Median, Mean-Std, Min으로 결합했으며, Mean이 대체로 가장 좋거나 비슷했다.

- Mean-Std는 $R=\mu-\sigma$로 disagreement가 큰 답변의 reward를 낮추는 pessimistic aggregation이다.

- 모든 RM이 같은 shortcut을 공유하면 $\sigma$가 충분히 커지지 않아 이 penalty가 제대로 작동하지 않는다.

- 따라서 pretrain ensemble은 개별 오류는 줄였지만, RM들이 공유하는 오류에는 여전히 취약했다. (4. 한계점에서 계속)

## 4. 한계점

3개의 과제에 대해서,

- Helpfulness 과제에서는 list형식이 과도하게 늘어났다

- TL;DR 과제에서는 요약 길이가 증가하고 원문 복사가 늘었다

- XSum/NLI 과제에서는 요약 길이가 감소하고 구체적 정보가 줄어들었다

이는 앙상블에 사용되는 모델들이 공통적으로 해당 성향에 reward hack되는 경향이 있을 때 해당 reward hack을 방어하지 못하기 때문에 발생한다.

## 5. 결론

pre training 과정과 finetuning 과정에서 여러 모델을 만들어서 ensemble을 적용하는 것은 각 rm에 이상하게 나타나는 오류는 평균화 할 수 있지만 공통적으로 나타나는 reward hack은 방어할 수 없다.