<template>
	<div class="container mx-auto px-4 py-8">
		<div class="max-w-4xl mx-auto">
			<div class="text-center mb-8">
				<h1 class="text-4xl font-bold text-gray-900 mb-4">
					미디어 품질 비교 학습 도구
				</h1>
				<p class="text-xl text-gray-600">
					오디오, 이미지, 비디오의 품질 차이를 직접 체험해보세요
				</p>
			</div>

			<div class="grid grid-cols-1 md:grid-cols-3 gap-6">
				<UCard class="hover:shadow-lg transition-shadow">
					<template #header>
						<div class="flex items-center gap-3">
							<UIcon
								name="i-lucide-headphones"
								class="w-6 h-6 text-green-500"
							/>
							<h3 class="text-lg font-semibold">황금귀 테스트</h3>
						</div>
					</template>

					<p class="text-gray-600 mb-4">
						다양한 음성을 들어보고 차이를 구분할 수 있는지 테스트해보세요.
					</p>

					<template #footer>
						<UButton to="/golden-ear" block icon="i-lucide-play">
							테스트 시작
						</UButton>
					</template>
				</UCard>

				<UCard class="hover:shadow-lg transition-shadow">
					<template #header>
						<div class="flex items-center gap-3">
							<UIcon name="i-lucide-image" class="w-6 h-6 text-green-500" />
							<h3 class="text-lg font-semibold">벡터 vs 비트맵</h3>
						</div>
					</template>

					<p class="text-gray-600 mb-4">
						벡터와 비트맵 이미지의 차이점을 확대해서 직접 비교해보세요.
					</p>

					<template #footer>
						<UButton to="/image-comparison" block icon="i-lucide-zoom-in">
							비교해보기
						</UButton>
					</template>
				</UCard>

				<UCard class="hover:shadow-lg transition-shadow">
					<template #header>
						<div class="flex items-center gap-3">
							<UIcon name="i-lucide-video" class="w-6 h-6 text-green-500" />
							<h3 class="text-lg font-semibold">프레임레이트 비교</h3>
						</div>
					</template>

					<p class="text-gray-600 mb-4">
						다양한 프레임레이트의 영상을 비교하며 차이점을 학습하세요.
					</p>

					<template #footer>
						<UButton to="/framerate-comparison" block icon="i-lucide-settings">
							비교해보기
						</UButton>
					</template>
				</UCard>
			</div>

			<div class="mt-12 text-center">
				<UAlert
					icon="i-lucide-info"
					title="학습 tip"
					description="각 테스트를 순서대로 진행하면서 미디어 품질에 대한 이해도를 높여보세요!"
				/>
			</div>
		</div>
	</div>
</template>

<script setup lang="ts"></script>
