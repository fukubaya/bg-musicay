<script lang="ts">
import { ref, defineComponent, onMounted } from "vue";
import imgGray from "./assets/gray.png";
import imgYellow from "./assets/yellow.png";
import imgBlue from "./assets/blue.png";
import imgRed from "./assets/red.png";
import imgPurple from "./assets/purple.png";
import imgPink from "./assets/pink.png";
import imgGreen from "./assets/green.png";

const SONG_LIST = [
   "OiSa",
   "わたし、恋始めたってよ！",
   "YOIMIYA",
   "御祭sawagi",
   "(6位)さがしもの",
   "和・華・蘭",
   "沸く星",
   "Bright & Breezy",
   "南風音頭",
   "禊 the MUSIC",
   "虹ノ湊",
   "FREEな波に乗って",
   "ばりかたプライド",
   "スウィンギタイ",
   "MILLION SUMMERS",
   "OTOMEdeshite",
   "でぃすたんす",
   "Dancer in the night",
   "ジャン！ジャン！ジャン！",
   "ありがとーと",
   "Dear My Blues",
   "Just mean it!",
   "Over",
   "Happy",
   "BDM",
   "Killer Killer Smile",
   "6STARS",
   "崇シ増シ×××物語",
   "己MYself",
   "MEGRRY GO ROUND",
   "無敵のビーナス",
   "ハカタダンスホールベイベー",
   "スターダスト",
   "Number Shot",
   "bye bye bye",
   "(7位)おっしょい！",
   "ばってん少女。",
   "びびび美少女",
   "夢のスコール",
   "STORM!",
   "(8位)ますとばい！",
   "とーと",
   "コトバテニス",
   "(9位)乙女ノ手札",
   "よかよかダンス",
   "(10位)すぺしゃるでい",
   "ころりん HAPPY FANTASY",
   "フレッ！フレッ！フレンズ",
   "君の手",
   "でてこいとびきりZENKAIパワー",
   "知っとっちゃん",
   "さよならDESTINY",
   "いざゆけ若鷹軍団",
   "アツか夏きたばい！",
   "夢のキャンバス",
 ];

const COLORS = [
   "gray",
   "yellow" ,
   "blue",
   "red",
   "purple",
   "pink",
   "green",
 ];

 export default defineComponent({
   setup(){
     const songList = SONG_LIST;
     const colors = COLORS;
     const canvasRef = ref<HTMLCanvasElement>();

     const name = ref<string>("");
     const color = ref<string>("gray");
     const img = ref<HTMLImageElement>(new Image());
     const rank1 = ref<string>("");
     const rank2 = ref<string>("");
     const rank3 = ref<string>("");
     const rank4 = ref<string>("");
     const rank5 = ref<string>("");
     
     const clearCanvas = () => {
       if (canvasRef.value !== undefined) {
         const ctx = canvasRef.value.getContext("2d");
         ctx?.clearRect(0, 0, canvasRef.value.width, canvasRef.value.height);
       }
     };
     const loadImage = () => {
       img.value.onload = () => {
         clearCanvas();
         if (canvasRef.value !== undefined) {
           canvasRef.value.width = img.value.width;
           canvasRef.value.height = img.value.height;
         }         
         drawImage();
       };
       switch(color.value){
   case "yellow":
     img.value.src = imgYellow;
     break;
   case "blue":
     img.value.src = imgBlue;
     break;
   case "red":
     img.value.src = imgRed;
     break;
   case "purple":
     img.value.src = imgPurple;
     break;
   case "pink":
     img.value.src = imgPink;
     break;
   case "green":
     img.value.src = imgGreen;
     break;
   default:
     img.value.src = imgGray;
     break;
       }

     }
     const drawImage = () => {
       const ctx = canvasRef.value?.getContext("2d");
       if (ctx == null) {
         return;
       }

       ctx.drawImage(img.value, 0, 0);
       
       // name
       ctx.font = "bold 87px sans-serif";
       ctx.textAlign = "center";
       ctx.textBaseline = "top";
       ctx.fillText(name.value, 11 + (711/2) ,20, 711);
       
       ctx.textAlign = "left";
       const rankWidth = 761;
       ctx.fillText(rank1.value, 177, 142, rankWidth);
       ctx.fillText(rank2.value, 177, 260, rankWidth);
       ctx.fillText(rank3.value, 177, 378, rankWidth);
       ctx.fillText(rank4.value, 177, 496, rankWidth);
       ctx.fillText(rank5.value, 177, 615, rankWidth);
     };

     onMounted(()=>{
      loadImage();
    });

     const downloadImage = () => {
       if (canvasRef.value === undefined) {
         return;
       }

       const mime = "image/png";
       const base64 = canvasRef.value.toDataURL(mime);
       const bin = atob(base64.split(",")[1]);
       const buf = new Uint8Array(bin.length);
       for (let i = 0; i < bin.length; i++) {
         buf[i] = bin.charCodeAt(i);
       }
       const blob = new Blob([buf.buffer], { type: mime });
       const data = window.URL.createObjectURL(blob);

       const a = document.createElement("a");
       a.download = "ranking.png";
       a.href = data;
       a.target = "_blank";
       document.body.appendChild(a);
       a.click();
       a.remove();
     };

     const openTwitter = () => {
       const texts = [
         name.value + "のランキング予想",
         "1位:" + rank1.value,
         "2位:" + rank2.value,
         "3位:" + rank3.value,
         "4位:" + rank4.value,
         "5位:" + rank5.value
       ];
       const hashtags = [
         "ばってん少女隊",
         "ばっしょー",
         "BATTENGIRLS",
         "BATTEN音楽の日"
       ];

       const text = texts.join("\n") + "\n";
       const hashtag = hashtags.join(",");
       const url = "https://twitter.com/intent/tweet?text=" + encodeURI(text) + "&hashtags=" + encodeURI(hashtag);
       window.open(url, "_blnak");
     };

    return {
      name,
      color,
      colors,
      rank1,
      rank2,
      rank3,
      rank4,
      rank5,
      songList,
      canvasRef,
      loadImage,
      drawImage,
      downloadImage,
      openTwitter,
    };
  }   
});
</script>

<template>
  <v-app id="bg-musicday">
    <v-app-bar
      density="compact"
      color="primary"
    >
      <v-app-bar-title>BATTEN音楽の日 ランキング予想</v-app-bar-title>
    </v-app-bar>

    <v-main>
      <v-container>
        <v-row>
          <v-col
            cols="12"
            sm="12"
            md="6"
          >
            <canvas
              ref="canvasRef"
              class="result"
            />
          </v-col>
          <v-col
            cols="12"
            sm="12"
            md="6"
          >
            <v-row>
              <v-col
                cols="6"
                xm="6"
                md="6"
              >
                <v-select
                  v-model="color"
                  label="色"
                  :items="colors"
                  @update:model-value="loadImage"
                />
              </v-col>

              <v-col
                cols="6"
                xm="6"
                md="6"
              >
                <v-btn
                  icon="mdi-download"
                  @click="downloadImage"
                />
                <v-btn
                  icon="mdi-twitter"
                  @click="openTwitter"
                />
              </v-col>
            </v-row>
  
            <v-text-field
              v-model="name"
              label="なまえ"
              @change="drawImage"
              @keyup="drawImage"
            />
            <v-select
              v-model="rank1"
              :items="songList"
              density="compact"
              label="1位"
              @update:model-value="drawImage"
            />
            <v-select
              v-model="rank2"
              :items="songList"
              density="compact"
              label="2位"
              @update:model-value="drawImage"
            />
            <v-select
              v-model="rank3"
              :items="songList"
              density="compact"
              label="3位"
              @update:model-value="drawImage"
            />
            <v-select
              v-model="rank4"
              :items="songList"
              density="compact"
              label="4位"
              @update:model-value="drawImage"
            />
            <v-select
              v-model="rank5"
              :items="songList"
              density="compact"
              label="5位"
              @update:model-value="drawImage"
            />
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<style scoped>
canvas.result {
  width: 100%;
}
</style>
