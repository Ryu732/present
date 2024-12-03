<template>
	<v-app>
		<v-main>
			<v-container>
				<div class="d-flex align-center justify-center fill-height">
					<v-btn color="primary">
						<v-icon>mdi-power</v-icon>
						カワウソ認証システムを起動
						<v-dialog v-model="isKawauso" activator="parent">
							<v-card prepend-icon="mdi-shield-check" title="カワウソ認証">
								<v-divider thickness="1"></v-divider>
								<v-card-text>
									カワウソ認証が必要です。<br />
									写真のカワウソの名前を入力してください。
									<v-img :src="imgUsosan" />
									<v-spacer />
									<v-text-field v-model="kawausoInput" label="カワウソの名前" />
								</v-card-text>
								<v-card-actions>
									<v-btn @click="kawausoAuth" color="primary">送信</v-btn>
								</v-card-actions>
							</v-card>
						</v-dialog>
					</v-btn>
					<v-dialog v-model="isAuth" persistent>
						<v-card>
							<v-card-title>認証成功!</v-card-title>
							<v-card-text>
								おめでとうございます<br />
								ばなながもらえます。
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
const imgUsosan = require('./assets/usosan.jpg');
const imgBanana = require('./assets/banana.jpg');

const isKawauso = ref<boolean>(false);
const isAuth = ref<boolean>(false);
const kawausoName = ref<string>('ウソさん');
const kawausoInput = ref<string>('');

function kawausoAuth() {
	if (kawausoName.value === kawausoInput.value) {
		isKawauso.value = false;
		isAuth.value = true;
	} else {
		alert('名前が違います。\nひらがなとカタカナに注意して入力してください。');
	}
}
</script>

<style scoped>
.v-container {
	height: 100%;
}
</style>
