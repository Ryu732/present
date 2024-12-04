<template>
	<v-app>
		<v-main>
			<v-container>
				<div class="d-flex align-center justify-center fill-height">
					<v-btn color="primary" v-show="!isAuth" size="large" rounded="xl">
						<div>
							<v-icon>mdi-power</v-icon>
							KWUS認証システムを起動
						</div>
						<v-dialog v-model="isKawauso" activator="parent">
							<v-card>
								<v-card-title class="d-flex align-center">
									<v-icon start color="primary">mdi-shield-check</v-icon>
									KWUS認証
								</v-card-title>
								<v-divider thickness="1"></v-divider>
								<v-card-text>
									<p>カワウソ認証が必要です。</p>
									<p>写真のカワウソの名前を入力してください。</p>
									<br />
									<v-img :src="imgUsosan" />
									<v-spacer />
									<v-text-field v-model="kawausoInput" label="カワウソの名前" />
								</v-card-text>
								<v-card-actions>
									<v-btn @click="kawausoAuth" color="primary" variant="flat">送信</v-btn>
								</v-card-actions>
							</v-card>
						</v-dialog>
					</v-btn>
					<div v-if="isLoading && isAuth" class="d-flex align-center justify-center fill-height loading">
						<v-progress-circular :model-value="loadingValue" :rotate="360" :size="130" :width="15"
							color="#FFD700">
							{{ loadingValue }}%
						</v-progress-circular>
						<p class="loading-text">認証中...</p>
					</div>
					<v-dialog v-model="isAuth" persistent v-if="!isLoading">
						<v-card v-if="!isLoading">
							<v-card-title class="d-flex align-center">
								<v-icon start color="success">mdi-check-circle</v-icon>
								認証成功
							</v-card-title>

							<v-divider></v-divider>

							<v-card-text>
								<p>おめでとうございます。</p>
								<p>ばなながもらえます。</p>
								<br />
								<v-img :src="imgBanana" />
							</v-card-text>
						</v-card>
					</v-dialog>
				</div>
			</v-container>
		</v-main>
	</v-app>


</template>

<script lang="ts" setup>
import { ref } from 'vue';
import confetti from 'canvas-confetti';

// 画像の読み込み
const imgUsosan = require('./assets/usosan.jpg');
const imgBanana = require('./assets/banana.jpg');

// 状態管理
const isKawauso = ref<boolean>(false);
const isAuth = ref<boolean>(false);
const isLoading = ref<boolean>(false);

// ローディング時間
const loadingValue = ref<number>(0);

// カワウソ認証
const kawausoName = ref<string>('ウソさん');
const kawausoInput = ref<string>('');

function kawausoAuth() {
	let loadingTimer: ReturnType<typeof setInterval>;

	// カワウソ認証
	if (kawausoName.value === kawausoInput.value) {
		isKawauso.value = false;
		isAuth.value = true;
		isLoading.value = true;
		loadingValue.value = 0;

		// ローディング
		loadingTimer = setInterval(() => {
			if (loadingValue.value >= 100) {
				clearInterval(loadingTimer);
				isLoading.value = false;

				// 紙吹雪を発射
				confetti({
					particleCount: 100,
					spread: 70,
					origin: { y: 0.6 }
				});
				return;
			}
			loadingValue.value += 2;
		}, 100);
	} else {
		// カワウソ認証失敗
		alert('名前が違います。\nひらがなとカタカナに注意して入力してください。');
	}
}
</script>

<style scoped>
.v-container {
	height: 100%;
}

.loading {
	flex-direction: column;
	gap: 16px;
}

.loading-text {
	font-size: 1.3rem;
	margin-top: 16px;
	color: #333;
}
</style>
