<template>
	<div class="container mx-auto px-4 py-8">
		<div class="max-w-4xl mx-auto">
			<div class="text-center mb-8">
				<h1 class="text-3xl font-bold text-gray-900 mb-4">황금귀 테스트</h1>
				<p class="text-lg text-gray-600">
					다양한 비트레이트의 오디오를 들어보고 차이를 구분해보세요
				</p>
			</div>

			<div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
				<!-- 저음질 오디오 -->
				<UCard>
					<template #header>
						<h3 class="text-xl font-semibold flex items-center gap-2">
							<UIcon name="i-lucide-volume-1" class="w-5 h-5 text-red-500" />
							저음질 (64kbps)
						</h3>
					</template>

					<div class="space-y-4">
						<audio
							ref="lowQualityAudio"
							class="w-full"
							controls
							preload="metadata"
						>
							<source src="/audio/sample-64kbps.mp3" type="audio/mpeg" />
							브라우저가 오디오를 지원하지 않습니다.
						</audio>

						<div class="text-sm text-gray-600">
							<p><strong>특징:</strong></p>
							<ul class="list-disc list-inside mt-2 space-y-1">
								<li>압축률이 높아 파일 크기가 작음</li>
								<li>고음역대의 디테일 손실</li>
								<li>압축 아티팩트가 들릴 수 있음</li>
							</ul>
						</div>
					</div>
				</UCard>

				<!-- 고음질 오디오 -->
				<UCard>
					<template #header>
						<h3 class="text-xl font-semibold flex items-center gap-2">
							<UIcon name="i-lucide-volume-2" class="w-5 h-5 text-green-500" />
							고음질 (320kbps)
						</h3>
					</template>

					<div class="space-y-4">
						<audio
							ref="highQualityAudio"
							class="w-full"
							controls
							preload="metadata"
						>
							<source src="/audio/sample-320kbps.mp3" type="audio/mpeg" />
							브라우저가 오디오를 지원하지 않습니다.
						</audio>

						<div class="text-sm text-gray-600">
							<p><strong>특징:</strong></p>
							<ul class="list-disc list-inside mt-2 space-y-1">
								<li>높은 비트레이트로 원음에 가까운 품질</li>
								<li>풍부한 고음역대와 디테일</li>
								<li>더 큰 파일 크기</li>
							</ul>
						</div>
					</div>
				</UCard>
			</div>

			<!-- 테스트 섹션 -->
			<UCard class="mt-8">
				<template #header>
					<h3 class="text-xl font-semibold flex items-center gap-2">
						<UIcon name="i-lucide-headphones" class="w-5 h-5 text-blue-500" />
						블라인드 테스트
					</h3>
				</template>

				<div class="space-y-6">
					<p class="text-gray-700">
						아래 두 오디오 중 어느 것이 더 높은 음질인지 맞춰보세요!
					</p>

					<div class="grid grid-cols-1 md:grid-cols-2 gap-6">
						<div class="text-center">
							<h4 class="text-lg font-medium mb-3">샘플 A</h4>
							<audio class="w-full mb-4" controls preload="metadata">
								<source :src="testAudio.sampleA" type="audio/mpeg" />
							</audio>
							<UButton
								@click="selectAnswer('A')"
								:variant="selectedAnswer === 'A' ? 'solid' : 'outline'"
								:color="selectedAnswer === 'A' ? 'green' : 'gray'"
								block
							>
								샘플 A가 더 좋음
							</UButton>
						</div>

						<div class="text-center">
							<h4 class="text-lg font-medium mb-3">샘플 B</h4>
							<audio class="w-full mb-4" controls preload="metadata">
								<source :src="testAudio.sampleB" type="audio/mpeg" />
							</audio>
							<UButton
								@click="selectAnswer('B')"
								:variant="selectedAnswer === 'B' ? 'solid' : 'outline'"
								:color="selectedAnswer === 'B' ? 'green' : 'gray'"
								block
							>
								샘플 B가 더 좋음
							</UButton>
						</div>
					</div>

					<div class="text-center" v-if="selectedAnswer">
						<UButton
							@click="checkAnswer"
							:disabled="!selectedAnswer || showResult"
							icon="i-lucide-check"
							size="lg"
						>
							정답 확인
						</UButton>
					</div>

					<UAlert
						v-if="showResult"
						:icon="isCorrect ? 'i-lucide-check-circle' : 'i-lucide-x-circle'"
						:color="isCorrect ? 'green' : 'red'"
						:title="isCorrect ? '정답입니다!' : '틀렸습니다.'"
						:description="resultMessage"
					/>

					<div class="text-center" v-if="showResult">
						<UButton
							@click="newTest"
							variant="outline"
							icon="i-lucide-refresh-cw"
						>
							새로운 테스트
						</UButton>
					</div>
				</div>
			</UCard>

			<!-- 학습 내용 -->
			<UCard class="mt-8">
				<template #header>
					<h3 class="text-xl font-semibold flex items-center gap-2">
						<UIcon name="i-lucide-book-open" class="w-5 h-5 text-purple-500" />
						오디오 품질에 대해 알아보기
					</h3>
				</template>

				<div class="prose max-w-none">
					<h4>비트레이트란?</h4>
					<p>
						비트레이트는 초당 전송되는 데이터의 양을 나타내며, 일반적으로
						kbps(kilobits per second) 단위로 측정됩니다. 높은 비트레이트일수록
						더 많은 오디오 정보를 포함하여 음질이 좋아집니다.
					</p>

					<h4>일반적인 비트레이트 품질</h4>
					<ul>
						<li><strong>64kbps:</strong> 낮은 음질, 음성 통화 수준</li>
						<li><strong>128kbps:</strong> 표준 음질, 일반적인 MP3</li>
						<li><strong>192kbps:</strong> 좋은 음질</li>
						<li><strong>320kbps:</strong> 매우 좋은 음질, CD 수준</li>
						<li><strong>무손실:</strong> 원본과 동일한 품질 (FLAC, WAV 등)</li>
					</ul>

					<h4>황금귀란?</h4>
					<p>
						음질의 미세한 차이를 구분할 수 있는 뛰어난 청각 능력을 가진 사람을
						'황금귀'라고 합니다. 훈련을 통해 오디오 품질에 대한 민감도를 높일 수
						있습니다.
					</p>
				</div>
			</UCard>

			<PageNavigation
				:next-page="{ path: '/image-comparison', label: '이미지 비교' }"
			/>
		</div>
	</div>
</template>

<script setup lang="ts">
import { ref, reactive } from "vue";
import PageNavigation from "../components/PageNavigation.vue";

const selectedAnswer = ref("");
const showResult = ref(false);
const isCorrect = ref(false);
const resultMessage = ref("");

const testAudio = reactive({
	sampleA: "/audio/sample-64kbps.mp3",
	sampleB: "/audio/sample-320kbps.mp3",
	correctAnswer: "B",
});

const selectAnswer = (answer: string) => {
	selectedAnswer.value = answer;
};

const checkAnswer = () => {
	isCorrect.value = selectedAnswer.value === testAudio.correctAnswer;
	showResult.value = true;

	if (isCorrect.value) {
		resultMessage.value =
			"샘플 B가 320kbps로 더 높은 음질입니다. 고음역대의 디테일과 전반적인 음질이 더 풍부합니다.";
	} else {
		resultMessage.value =
			"샘플 B가 정답입니다. 320kbps는 64kbps보다 5배 높은 비트레이트로 더 많은 오디오 정보를 담고 있습니다.";
	}
};

const newTest = () => {
	// 랜덤하게 A/B 순서 바꾸기
	const shouldSwap = Math.random() > 0.5;

	if (shouldSwap) {
		testAudio.sampleA = "/audio/sample-320kbps.mp3";
		testAudio.sampleB = "/audio/sample-64kbps.mp3";
		testAudio.correctAnswer = "A";
	} else {
		testAudio.sampleA = "/audio/sample-64kbps.mp3";
		testAudio.sampleB = "/audio/sample-320kbps.mp3";
		testAudio.correctAnswer = "B";
	}

	selectedAnswer.value = "";
	showResult.value = false;
	isCorrect.value = false;
	resultMessage.value = "";
};
</script>
