<template>

  <nav
    class="position-fixed  top-0 left-0 d-flex justify-content-between align-items-center px-5"
    style="width: 100vw; height: 80px; background-color: rgba(56, 189, 248);z-index: 5;    backdrop-filter: blur(10px);"
  >
    <div class="logo">
      <img src="/img/logo.png" alt="" width="60" height="60">
    </div>
    <ul class="d-flex align-items-center m-0" style="list-style-type: none">
      <li class="me-2">Home</li>
      <li class="me-2">Home</li>
      <li class="me-2">Home</li>
      <li class="me-2">Home</li>
      
    </ul>
  </nav>
  <div class="container" style="margin-top: 7.5rem">
    <div class="d-flex justify-content-center">
      
      <library-song
      :songs="filteredSongs"
      @select-song="selectSong"
      :filterSong="filterSong"
      />
      <div style="background-color: #e0f2fe" class="p-4 rounded me-5">
        <div style="max-width: 300px; height: 300px">
          <img :src="currentSong.img" alt="" class="w-100 h-100" />
        </div>
        <div class="font-weight-bold my-2" style="font-size: 30px">
          {{ currentSong.title }}
        </div>
        <div class="mb-4">{{ currentSong.single }}</div>
        <audio
          ref="audio"
          :src="currentSong.src"
          @timeupdate="updateTime"
          @ended="nextSong"
        ></audio>

        <div class="controls d-flex gap-4 justify-content-center">
          <button
            class="m-0 border-0 rounded rounded-lg p-1 mx-2"
            style="background-color: #f5f5f5; font-size: 30px"
            @click="prevSong"
          >
            ⏮️
          </button>
          <button
            class="m-0 border-0 rounded rounded-lg p-1 mx-2"
            style="background-color: #f5f5f5; font-size: 30px"
            @click="togglePlay()"
          >
            {{ isPlaying ? "⏸️" : "▶️" }}
          </button>
          <button
            class="m-0 border-0 rounded rounded-lg p-1 mx-2"
            style="background-color: #f5f5f5; font-size: 30px"
            @click="nextSong"
          >
            ⏭️
          </button>
        </div>

        <div class="progress">
          <input
            type="range"
            min="0"
            :max="duration"
            step="0.1"
            v-model="currentTime"
            @input="seek"
          />
          <div
            style="width: 30%; padding-left: 10px; background-color: #e0f2fe"
          >
            {{ formatTime(currentTime) }} / {{ formatTime(duration) }}
          </div>
        </div>
      </div>
      <div
        v-html="currentSong.music"
        style="overflow-y: scroll; height: 33rem;"
        class="ms-5"
      ></div>
      <form action="">
        <div>
          <label for="code">Enter code song</label>
          <input type="text" name="" id="" v-model="code">
          <button @click.prevent="submitCode($event)">Submit</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import LibrarySong from "./components/LibrarySong.vue";

export default {
  components: {
    LibrarySong,
  },
  data() {
    return {
      songs: [
        {
          id: 0,
          title: "アイのウイルス",
          single: "Takane no nadeshiko",
          img: "./img/tk.png",
          src: "./music/tk.mp3",
          music: `
                <div>アイのウィルス (Ai no Uirusu)</div>
                <div>ไวรัสแห่งความรัก</div>
                <br>
                <div>どうして好きになってしまったのアナタを</div>
                <div>(Doushite suki ni natte shimatta no anata wo)</div>
                <div>ทำไมถึงเผลอรักคุณไปแบบนี้</div>
                <div>嗤笑う世界 愛は不条理で嫌いだ</div>
                <div>(Warau sekai ai wa fujouri de kirai da)</div>
                <div>โลกที่หัวเราะเยาะกัน ความรักช่างไร้เหตุผลและน่ารังเกียจ</div>
                <div>子供のままじゃ生きていけない</div>
                <div>(Kodomo no mama ja ikite ikenai)</div>
                <div>เป็นเด็กตลอดไปคงอยู่ไม่ได้</div>
                <div>触れてほしい、愛してよ。ねぇ？</div>
                <div>(Furete hoshii, aishite yo. Nee?)</div>
                <div>อยากให้คุณสัมผัส อยากให้คุณรัก... ใช่ไหม?</div>
                <br>
              
                <div>戦うことやめない</div>
                <div>(Tatakau koto yamenai)</div>
                <div>ฉันจะไม่หยุดต่อสู้</div>
                <div>抗うことやめない</div>
                <div>(Aragau koto yamenai)</div>
                <div>จะไม่หยุดต่อต้าน</div>
                <div>後悔なんてないから</div>
                <div>(Koukai nante nai kara)</div>
                <div>ไม่มีคำว่าเสียใจ</div>
                <div>醜い姿でもいい</div>
                <div>(Minikui sugata demo ii)</div>
                <div>แม้จะดูน่าเกลียดก็ไม่เป็นไร</div>
                <br>
                <div>どうして私じゃダメだったの？</div>
                <div>(Doushite watashi ja dame datta no?)</div>
                <div>ทำไมฉันถึงไม่ใช่คนที่คุณเลือก?</div>
                <div>痛いよ</div>
                <div>(Itai yo)</div>
                <div>มันเจ็บปวดเหลือเกิน</div>
                <div>弄ぶ世界 愛を侮辱した世界だ</div>
                <div>(Motenasubu sekai ai wo bujoku shita sekai da)</div>
                <div>โลกที่ล้อเล่นกับความรัก โลกที่ดูถูกความรัก</div>
                <br>
                <div>大人の定義</div>
                <div>(Otona no teigi)</div>
                <div>คำจำกัดความของ "ผู้ใหญ่"</div>
                <div>利用したって...</div>
                <div>(Riyou shitatte...)</div>
                <div>คือการใช้ประโยชน์...</div>
                <div>この瞬間、愛してよ。ねぇ？</div>
                <div>(Kono shunkan, aishite yo. Nee?)</div>
                <div>ในชั่วขณะนี้ โปรดรักฉัน... ได้ไหม?</div>
                <br>
                <div>逃げないから重ねて</div>
                <div>(Nigenai kara kasanete)</div>
                <div>ฉันจะไม่หนี จะยืนหยัด</div>
                <div>願いをただ重ねて</div>
                <div>(Negai wo tada kasanete)</div>
                <div>จะขอแค่เพียงภาวนา</div>
                <div>覚悟はもうできてる</div>
                <div>(Kakugo wa mou dekiteru)</div>
                <div>ความมุ่งมั่น ฉันมีอยู่แล้ว</div>
                <div>醜い姿でもいい</div>
                <div>(Minikui sugata demo ii)</div>
                <div>แม้จะดูน่าเกลียดก็ไม่เป็นไร</div>
                <br>
                <div>もっとアナタの話す声を</div>
              <div>(Motto anata no hanasu koe wo)</div>
              <div>อยากฟังเสียงของคุณให้มากกว่านี้</div>

              <div>もっとアナタの笑う顔を</div>
              <div>(Motto anata no warau kao wo)</div>
              <div>อยากเห็นรอยยิ้มของคุณให้มากกว่านี้</div>

              <div>もっとアナタとの思い出を</div>
              <div>(Motto anata to no omoide wo)</div>
              <div>อยากเก็บความทรงจำกับคุณให้มากกว่านี้</div>

              <div>もっと…</div>
              <div>(Motto…)</div>
              <div>มากกว่านี้...</div>
              <br>
              <div>戦うことやめない</div>
              <div>(Tatakau koto yamenai)</div>
              <div>ฉันจะไม่หยุดต่อสู้</div>

              <div>抗うことやめない</div>
              <div>(Aragau koto yamenai)</div>
              <div>จะไม่หยุดต่อต้าน</div>

              <div>後悔なんてないから</div>
              <div>(Koukai nante nai kara)</div>
              <div>เพราะไม่มีอะไรให้เสียใจ</div>

              <div>醜い姿でもいい</div>
              <div>(Minikui sugata demo ii)</div>
              <div>ถึงจะดูน่าเกลียดก็ไม่เป็นไร</div>

              <div>逃げないから重ねて</div>
              <div>(Nigenai kara kasanete)</div>
              <div>ฉันจะไม่หนีไปไหน แค่ขอเพิ่มมันอีกครั้ง</div>

              <div>願いをただ重ねて</div>
              <div>(Negai wo tada kasanete)</div>
              <div>แค่ขอซ้อนคำอธิษฐานเอาไว้</div>

              <div>覚悟はもうできてる</div>
              <div>(Kakugo wa mou dekiteru)</div>
              <div>ฉันเตรียมใจไว้แล้ว</div>

              <div>醜い姿でもいい</div>
              <div>(Minikui sugata demo ii)</div>
              <div>ถึงจะดูน่าเกลียดก็ไม่เป็นไร</div>
  `,
          active: true,
          type:  'j-pop',
          status:"use"
          
        },

        {
          id: 1,
          title: "มึงเขียนเดี๋ยวนี้!",
          single: "Painekung Ai",
          img: "./img/dev.jpg",
          src: "./music/dev.mp3",
          active: false,
          type: "ai",
          music:
            "<div>Verse 1</div><div>“อย่ามาพูดมาก ถ้าไม่ได้ช่วยเขียนโค้ด”</div><div>ชอบแค่พูดแต่ไม่ลงมือ คุณอย่ามาโชว์</div><div>คนมันจริง ใจนักเลงมันล้น</div><div>อยู่หน้าคอมทั้งคืน แต่ใจไม่เคยทน</div><br><div>Pre-Chorus</div><div>Compiler พัง ผมไม่สน</div><div>เปิด Terminal ใหม่แล้วลุยอีกจนวันพรุ่งนี้ั</div><div>ใครมาขวางงาน บอกไว้เลยจะโดน</div><div>มือซ้าย Debug มือขวาพร้อมจะปกป้องโค้ด</div><br><div>Chorus</div><div>ถ้าไม่เขียนให้คุณจะโดนต่อย!</div><div>อย่ามายืนงง ให้มันจบไปซักหน่อย</div><div>บั๊กตัวใหญ่แค่ไหนผมจัดให้</div><div>เพราะโลกนี้ต้องเคารพ Dev ตัวจริง</div><br><div>Verse 2</div><div>“เจ้านายบอก Deadline คือเมื่อวาน”</div><div>เอาแค่นั้น? ได้ เดี๋ยวจัดให้มันผ่าน</div><div>แต่ว่าช่วยไม่กวน ถ้าคุณไม่รู้จริง</div><div>เพราะ Dev แบบผม เขียนโค้ดจนลิงยังอาย</div><br><div>Pre-Chorus</div><div>ดึกแค่ไหนใจมันก็ยังวิ่ง</div><div>คาเฟอีนหมดก็เติม ใจไม่เคยนิ่ง</div><div>ใครพูดเยอะ ไม่ช่วยก็ถอยไป</div><div>สาย Dev แบบนี้ มันต้องใจใหญ่</div><br><div>Chorus</div><div>ถ้าไม่เขียนให้คุณจะโดนต่อย!</div><div>อย่ามายืนงง ให้มันจบไปซักหน่อย</div><div>บั๊กตัวใหญ่แค่ไหนผมจัดให้</div><div>เพราะโลกนี้ต้องเคารพ Dev ตัวจริง</div><br><div>Bridge</div><div>บั๊กมันเยอะ เอาให้มันจบ</div><div>สายตาโฟกัส โค้ดต้องครบ</div><div>ใครมาขวางผม จะโดนแน่</div><div>มือขวาเขียนโค้ด มือซ้ายพร้อมตบ!</div>",
          status:"use"
          },
        {
          id: 2,
          title: "I'M YOUR IDOL",
          single: "Takane no nadeshiko",
          img: "./img/m.jpeg",
          src: "./music/m.mp3",
          music: `
              <div>一つの歌声じゃダメだった</div>
              <div>(Hitotsu no utagoe ja dame datta)</div>
              <div>เสียงเพลงเดียวคงไม่พอ</div>

              <div>一つの歌声が寄り添った</div>
              <div>(Hitotsu no utagoe ga yorisotta)</div>
              <div>เสียงเพลงเดียวได้เข้ามาอยู่เคียงข้างกัน</div>

              <div>僕らは非力で無力かもしれない</div>
              <div>(Bokura wa hiryoku de muryoku kamo shirenai)</div>
              <div>พวกเราอาจจะอ่อนแอและไร้พลัง</div>

              <div>それでも歌おう世界に さあ</div>
              <div>(Soredemo utaou sekai ni saa)</div>
              <div>ถึงอย่างนั้นก็ร้องเพลงให้โลกฟังกันเถอะ เอาสิ</div>

              <div>手を取れ弱さだって愛せる</div>
              <div>(Te o tore yowasa datte aiseru)</div>
              <div>จับมือกันสิ แม้ความอ่อนแอก็รักได้</div>

              <div>失敗 痛みだって愛せる</div>
              <div>(Shippai itami datte aiseru)</div>
              <div>แม้ความผิดพลาด ความเจ็บปวดก็รักได้</div>

              <div>僕らの想いは重なり歌となる</div>
              <div>(Bokura no omoi wa kasanari uta to naru)</div>
              <div>ความรู้สึกของพวกเราจะซ้อนทับกันกลายเป็นเพลง</div>

              <div>誰かの心を灯せるように</div>
              <div>(Dareka no kokoro o tomoseru you ni)</div>
              <div>เพื่อที่จะจุดประกายในใจใครบางคน</div>
              <br>

              <div>休んだっていい</div>
              <div>(Yasundatte ii)</div>
              <div>พักผ่อนก็ได้</div>

              <div>寄り道したっていい</div>
              <div>(Yorimichi shitatte ii)</div>
              <div>แวะข้างทางก็ได้</div>

              <div>涙見せたっていい</div>
              <div>(Namida misetatte ii)</div>
              <div>จะร้องไห้ก็ได้</div>

              <div>我慢しなくていい</div>
              <div>(Gaman shinakute ii)</div>
              <div>ไม่ต้องอดทนก็ได้</div>

              <div>大丈夫だよ そばにいるよ</div>
              <div>(Daijoubu da yo soba ni iru yo)</div>
              <div>ไม่เป็นไร ฉันอยู่ข้างๆ เธอ</div>

              <div>君へのファンファーレ</div>
              <div>(Kimi e no fanfaare)</div>
              <div>เสียงแตรแสดงความยินดีแด่เธอ</div>

              <div>苦しみも悲しみも寂しさも</div>
              <div>(Kurushimi mo kanashimi mo sabishisa mo)</div>
              <div>ความทุกข์ ความเศร้า ความเหงา</div>

              <div>任せて</div>
              <div>(Makasete)</div>
              <div>ปล่อยให้ฉันรับไว้เอง</div>
              <br>
              <div>世界飾る愛のファンタジー</div>
              <div>(Sekai kazaru ai no fantajii)</div>
              <div>จินตนาการแห่งความรักที่ประดับประดาโลก</div>

              <div>言葉国境も越えて</div>
              <div>(Kotoba kokkyou mo koete)</div>
              <div>ข้ามพ้นคำพูดและพรมแดน</div>

              <div>魂揺らすハーモニー</div>
              <div>(Tamashii yurasu haamonii)</div>
              <div>เสียงประสานที่สั่นสะเทือนจิตวิญญาณ</div>

              <div>握れタクト 君のストーリー</div>
              <div>(Nigire takuto kimi no sutoorii)</div>
              <div>จับไม้บาตองไว้ เรื่องราวของเธอ</div>

              <div>五線譜で踊り出す</div>
              <div>(Gosenfu de odoridasu)</div>
              <div>เต้นรำไปบนบรรทัดห้าเส้น</div>

              <div>無限大のメロディー</div>
              <div>(Mugendai no merodii)</div>
              <div>ท่วงทำนองที่ไม่มีที่สิ้นสุด</div>

              <div>フィナーレ迎えはしない</div>
              <div>(Finaare mukae wa shinai)</div>
              <div>จะไม่พบกับฉากจบ</div>

              <div>幸せが溢れる今</div>
              <div>(Shiawase ga afureru ima)</div>
              <div>ความสุขที่ล้นปรี่ในตอนนี้</div>

              <div>きっと続く</div>
              <div>(Kitto tsuzuku)</div>
              <div>จะต้องดำเนินต่อไปอย่างแน่นอน</div>
              <br>
              <div>一人じゃ逆境に負けてた</div>
              <div>(Hitori ja gyakkyou ni maketeta)</div>
              <div>ถ้าอยู่คนเดียวคงแพ้ต่อความยากลำบาก</div>

              <div>一人じゃ感情も捨ててた</div>
              <div>(Hitori ja kanjou mo suteteta)</div>
              <div>ถ้าอยู่คนเดียวคงทิ้งความรู้สึกไปแล้ว</div>

              <div>僕らは埋め合い輝くことを知る</div>
              <div>(Bokura wa umeai kagayaku koto o shiru)</div>
              <div>พวกเรารู้ว่าจะเติมเต็มกันและกันและเปล่งประกาย</div>

              <div>だからさ歌おう世界に ほら</div>
              <div>(Dakara sa utaou sekai ni hora)</div>
              <div>เพราะฉะนั้นมาร้องเพลงให้โลกฟังกันเถอะ ดูสิ</div>

              <div>ぶつかる熱意だって持ってる</div>
              <div>(Butsukaru netsui datte motteru)</div>
              <div>พวกเรามีความกระตือรือร้นที่จะเข้าปะทะ</div>

              <div>折れないハートだって持ってる</div>
              <div>(Orenai haato datte motteru)</div>
              <div>พวกเรามีหัวใจที่ไม่ยอมแพ้</div>

              <div>僕らの願いは膨らみ夢となる</div>
              <div>(Bokura no negai wa fukurami yume to naru)</div>
              <div>ความปรารถนาของพวกเราจะเบ่งบานกลายเป็นความฝัน</div>

              <div>誰かの心を灯せるように</div>
              <div>(Dareka no kokoro o tomoseru you ni)</div>
              <div>เพื่อที่จะจุดประกายในใจใครบางคน</div>
              <br>
              <div>転んだっていい</div>
              <div>(Korondatte ii)</div>
              <div>ล้มก็ได้</div>

              <div>傷ができたっていい</div>
              <div>(Kizu ga dekitatte ii)</div>
              <div>มีบาดแผลก็ได้</div>

              <div>這いつくばっていい</div>
              <div>(Haitsukubatte ii)</div>
              <div>คลานไปก็ได้</div>

              <div>やり直せばいい</div>
              <div>(Yarinaoseba ii)</div>
              <div>เริ่มใหม่ก็ได้</div>

              <div>大丈夫だよ 味方なんだよ</div>
              <div>(Daijoubu da yo mikata nanda yo)</div>
              <div>ไม่เป็นไร ฉันอยู่ข้างๆ เธอ</div>

              <div>君へのファンファーレ</div>
              <div>(Kimi e no fanfaare)</div>
              <div>เสียงแตรแสดงความยินดีแด่เธอ</div>

              <div>楽しみも喜びも幸せも</div>
              <div>(Tanoshimi mo yorokobi mo shiawase mo)</div>
              <div>ความสนุก ความยินดี และความสุข</div>

              <div>任せて</div>
              <div>(Makasete)</div>
              <div>ปล่อยให้ฉันรับไว้เอง</div>
              <br>
              <div>世界駆ける愛のコンチェルト</div>
              <div>(Sekai kakeru ai no koncheruto)</div>
              <div>คอนแชร์โตแห่งความรักที่วิ่งไปรอบโลก</div>

              <div>君の花 照らし出す</div>
              <div>(Kimi no hana terashidasu)</div>
              <div>ดอกไม้ของเธอจะถูกส่องแสงออกมา</div>

              <div>太陽のパレード</div>
              <div>(Taiyou no pareedo)</div>
              <div>ขบวนพาเหรดของพระอาทิตย์</div>

              <div>握れタクト 君のストーリー</div>
              <div>(Nigire takuto kimi no sutoorii)</div>
              <div>จับไม้บาตองไว้ เรื่องราวของเธอ</div>

              <div>自由な音符 はしゃぐ</div>
              <div>(Jiyuu na onpu hasyagu)</div>
              <div>โน้ตดนตรีที่อิสระและกระโดดโลดเต้น</div>

              <div>心奪うメロディ</div>
              <div>(Kokoro ubau merodii)</div>
              <div>ทำนองที่ขโมยหัวใจ</div>

              <div>フィナーレ迎えはしない</div>
              <div>(Finaare mukae wa shinai)</div>
              <div>จะไม่พบกับฉากจบ</div>

              <div>美しすぎる世界は</div>
              <div>(Utsukushi sugiru sekai wa)</div>
              <div>โลกที่สวยเกินไปนั้น</div>

              <div>君のものだ</div>
              <div>(Kimi no mono da)</div>
              <div>เป็นของเธอ</div>
              <br>
              <div>人生という旅で</div>
              <div>(Jinsei to iu tabi de)</div>
              <div>ในชีวิตที่เป็นการเดินทาง</div>

              <div>出会う この奇跡</div>
              <div>(Deau kono kiseki)</div>
              <div>พบเจอปาฏิหาริย์นี้</div>

              <div>泣いてしまうくらい</div>
              <div>(Naite shimau kurai)</div>
              <div>จนถึงกับน้ำตาจะไหล</div>

              <div>君を愛してる</div>
              <div>(Kimi wo aishiteru)</div>
              <div>ฉันรักเธอ</div>

              <div>言葉国境も越えて</div>
              <div>(Kotoba kokkyou mo koete)</div>
              <div>ข้ามพรมแดนของคำพูด</div>

              <div>フィナーレ迎えはしない</div>
              <div>(Finaare mukae wa shinai)</div>
              <div>จะไม่พบกับฉากจบ</div>

              <div>君の胸に刻まれて</div>
              <div>(Kimi no mune ni kizamarete)</div>
              <div>สลักไว้ในใจของเธอ</div>

              <div>人から人へ繋がる</div>
              <div>(Hito kara hito e tsunagaru)</div>
              <div>จากคนหนึ่งสู่คนอื่นๆ</div>

              <div>幸せが溢れる今</div>
              <div>(Shiawase ga afureru ima)</div>
              <div>ตอนนี้ที่ความสุขล้นหลาม</div>

              <div>世界飾る愛のファンタジー</div>
              <div>(Sekai kazaru ai no fantajii)</div>
              <div>จินตนาการแห่งความรักที่ประดับประดาโลก</div>

              <div>魂揺らすハーモニー</div>
              <div>(Tamashii yurasu haamonii)</div>
              <div>เสียงประสานที่สั่นสะเทือนจิตวิญญาณ</div>

              <div>握れタクト 君のストーリー</div>
              <div>(Nigire takuto kimi no sutoorii)</div>
              <div>จับไม้บาตองไว้ เรื่องราวของเธอ</div>

              <div>五線譜で踊り出す</div>
              <div>(Gosenfu de odoridasu)</div>
              <div>เต้นรำไปบนบรรทัดห้าเส้น</div>

              <div>無限大のメロディー</div>
              <div>(Mugendai no merodii)</div>
              <div>ท่วงทำนองที่ไม่มีที่สิ้นสุด</div>

              <div>永久に続く</div>
              <div>(Eikyuu ni tsuzuku)</div>
              <div>ดำเนินต่อไปตลอดกาล</div>        
          `,
          active: false,
          type: "j-pop",
          status:"use"
        },
        {
          id: 3,
          title: "初恋のひと。",
          single: "Takane no nadeshiko",
          img: "./img/ha.png",
          src: "./music/ha.mp3",
          active: false,
          type: "j-pop",
          status:"delete",
          code:"tknn-pkk-snt"
        },
        {
          id: 4,
          title: "進んでいく人生",
          single: "Painekung Ai",
          img: "./img/chil.jpg",
          src: "./music/chill.mp3",
          active: false,
          type: "ai",
          status:"delete",
          code:"seas-ssse-pkk"
        },{
          id:5,
          title:"Lost Jigsaw",
          single:" Upim LANDOKMAI",
          img:"./img/lostjigsaw.png",
          src:"./music/lostjigsaw.mp3",
          type:"sad",
          status:'delete',
          code:"loveyou"
        },
        {
          id:6,
          title:"Song Maker",
          single:"Erisa Higashiyama",
          img:"./img/fff.jpg",
          src:"./music/fff.mp3",
          type:"j-pop",
          status:"delete",
          code:"richan-orange"
        },
        {
          id:7,
          title:"Song Maker",
          single:"Matsumoto Momona",
          img:"./img/Momo.jpg",
          src:"./music/Momo.mp3",
          type:"j-pop",
          status:"use",
          code:"sssssssss"
        }
      ],
       filteredSongs: [],
      currentIndex: 0,
      isPlaying: false,
      currentTime: 0,
      duration: 0,
      isSidebarOpen: true, // ใช้สำหรับเก็บสถานะของ sidebar
      index:0,
      code: '',
      keepCode:[]
    };
  },
  computed: {
    currentSong() {
      return this.songs[this.currentIndex];
    },
  },
  methods: {
    togglePlay() {
      const audio = this.$refs.audio;
      if (this.isPlaying) {
        audio.pause();
      } else {
        audio.play().catch((error) => {
          console.error("Error playing audio:", error);
        });
      }
      this.isPlaying = !this.isPlaying;
    },
    updateTime() {
      const audio = this.$refs.audio;
      this.currentTime = audio.currentTime;
      this.duration = audio.duration;
    },
    seek() {
      const audio = this.$refs.audio;
      audio.currentTime = this.currentTime;
    },
    formatTime(seconds) {
      const minutes = Math.floor(seconds / 60);
      const secs = Math.floor(seconds % 60);
      return `${minutes}:${secs < 10 ? "0" : ""}${secs}`;
    },
    prevSong() {
       this.index--
    
      if(this.index < 0){
        this.currentIndex = this.filteredSongs[this.filteredSongs.length-1].id
        this.index = this.filteredSongs.length - 1
      } else {
          this.currentIndex = this.filteredSongs[this.index].id
      }
     this.filteredSongs.forEach((song, i) => {
        song.active = song.id === this.currentIndex;
      });
      this.resetPlayer();
    },
    nextSong() {
      // this.currentIndex = (this.filteredSongs[0].id + 1) % this.filteredSongs.length;

       this.index++
       
      
       if(this.index > this.filteredSongs.length-1){
        this.currentIndex = this.filteredSongs[0].id
        this.index = 0
       } else {
        this.currentIndex = this.filteredSongs[this.index].id
       }
      
      this.filteredSongs.forEach((song, i) => {
        song.active = song.id === this.currentIndex;
      });

      this.resetPlayer();
    },
    resetPlayer() {
      const audio = this.$refs.audio;
      audio.pause();
      this.isPlaying = false;
      this.currentTime = 0;
      audio.load();

      audio.oncanplay = () => {
        this.togglePlay();
        audio.oncanplay = null;
      };
    },

    toggleSidebar() {
      this.isSidebarOpen = !this.isSidebarOpen; // เปลี่ยนสถานะการเปิด/ปิด
    },
    selectSong(index) {
      this.songs.forEach((song, i) => {
        song.active = i === index;
      });
      this.currentIndex = index;
      this.index = this.filteredSongs.findIndex(song => song.id === index)
      this.resetPlayer();
    },
    filterSong(type) {
           if (type) {
        this.filteredSongs = this.songs.filter((song) => song.type === type && song.status === "use");
      } else {
        this.filteredSongs = this.songs.filter((song)=> song.status === 'use');
      }
      this.filteredSongs.forEach((song)=> {
        song.active = song.id === this.currentIndex
      })
     
      this.selectSong(this.filteredSongs[0].id)
      this.resetPlayer();
      this.index = 0
    },
    submitCode(e){
      const check = false
      e.preventDefault()
      this.songs.forEach((songCode)=> {
        if(songCode.code === this.code){
          if(songCode.status !== 'use'){
             songCode.status = "use"
              this.filteredSongs = this.songs.filter((song)=> song.status === 'use');
              this.selectSong(this.filteredSongs[0].id)
              this.index = 0
              this.showAlert("Complete")
              return check = true
          } else {
            this.showAlert("คุณกรอกรหัสนี้ไปแล้ว\nกรุณากรอกใหม่อีกครั้ง")
            return check = true
          }
        }
      })
     if(!check){
      this.showAlert("Code Not found!!")
     }
    },
    showAlert(m) {
      // Use sweetalert2
      this.$swal(m);
    },
  },
   mounted() {
    // Default filter on mount
    this.filteredSongs = this.songs.filter((song)=> song.status === 'use');
    
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
}
.player {
  width: 300px;
  margin: auto;
  border: 1px solid #ccc;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}
.controls button {
  cursor: pointer;
}
.progress {
  margin-top: 20px;
}

/* ตั้งค่า input range ให้ดูเรียบง่าย */
input[type="range"] {
  -webkit-appearance: none;
  width: 100%;
  background: transparent; /* ลบพื้นหลังค่าเริ่มต้น */
  position: relative;
  background-color: #e0f2fe;
}
#hover-btn{
   background-color:  #020617;
}
#hover-btn:hover{
  background-color: #ec4899;
}
/* Track สีพื้นหลัง */
input[type="range"]::before {
  content: "";
  position: absolute;
  top: 50%;
  left: 0;
  width: 100%;
  height: 6px;
  background: red; /* สี Track ชั้นล่าง */
  border-radius: 4px;
  transform: translateY(-50%);
  z-index: 1; /* ให้อยู่ใต้ Track ด้านบน */
  z-index: -1;
}

/* Track สีด้านบน */
input[type="range"]::-webkit-slider-runnable-track {
  width: 100%;
  height: 6px;
  background: white; /* สี Track ชั้นบน */
  border-radius: 4px;
}

/* Thumb (จุดเลื่อน) */
input[type="range"]::-webkit-slider-thumb {
  -webkit-appearance: none;
  width: 16px;
  height: 16px;
  border-radius: 50%;
  background: #0ea5e9; /* สี Thumb */
  border: 1px solid #d3d3d3; /* ขอบ Thumb */
  box-shadow: 0 0 4px rgba(0, 0, 0, 0.2);
  cursor: pointer;
  margin-top: -5px; /* จัด Thumb ให้ตรงกลาง */
  z-index: 2; /* ให้อยู่เหนือ Track */
  z-index: 10;
}

/* Focus: ลบ Outline */
input[type="range"]:focus {
  outline: none;
}
</style>
