<template>
  <div>
    <div class='wraper-inner'>
      <section class='main-new-section'>
        <img src='images/main-banner-images.png'>
        <div class='container'>
          <div class='main-header-blk' style='z-index:1000;'>
            <div class='main-header-logo'>
              <img src='postcardwtf.gif' width='150' height='150'>
            </div>
            <div class='main-header-content'>
              <h6>This Dapp lets you send notes to the wallets of your friends as a NFT! It can be used to send thank
                you notes, holiday greetings, on chain messaging service, pranks and anything else you can think up! You
                can use emoticons! ❤️ [V 1.0]</h6>
            </div>
            <div class='main-header-actions'>
              <button class='btn wallet-select-btn'>Select Wallet</button>
            </div>
          </div>
          <div class='main-middle-block'>
            <div class='row'>
              <div class='col-md-6'>
                <div class='color-picker-block'>
                  <div class='form-group'>
                    <label>Background Color</label>
                    <div class='color-picker'>
                      <div id='first' class='bg-color selected' @click='toggleSelect(this)'></div>
                      <div id='second' class='bg-color' @click='toggleSelect(this)'></div>
                      <div id='third' class='bg-color' @click='toggleSelect(this)'></div>
                      <div id='fourth' class='bg-color' @click='toggleSelect(this)'></div>
                      <div id='fifth' class='bg-color' @click='toggleSelect(this)'></div>
                      <div id='custom' class='bg-color multicolor' @click='toggleSelect(this)'>
                        <input type='color' id='bgColorPicked' oninput='colorPick(this)' value='#eb4d4b'>
                      </div>
                    </div>
                  </div>
                  <div class='form-group'>
                    <label>Foreground Color</label>
                    <div class='color-picker'>
                      <div id='first' class='txt-color' @click='toggleSelectTxtColor(this)'></div>
                      <div id='second' class='txt-color' @click='toggleSelectTxtColor(this)'></div>
                      <div id='third' class='txt-color' @click='toggleSelectTxtColor(this)'></div>
                      <div id='fourth' class='txt-color' @click='toggleSelectTxtColor(this)'></div>
                      <div id='fifth' class='txt-color' @click='toggleSelectTxtColor(this)'></div>
                      <div id='custom' class='txt-color' @click='toggleSelectTxtColor(this)'>
                        <input type='color' id='txtColorPicked' oninput='txtColorPick(this)' value='#ffffff'>
                      </div>
                    </div>
                  </div>
                  <div class='form-group'>
                    <label>Write Your PostCard</label>
                    <textarea class='card-content' @keyup='writeText(this)' maxlength='144' rows='4'
                      placeholder='Write text here...'></textarea>
                  </div>
                  <span class='download-btn' @click='downloadim()'>
                    <!-- <i class='ri-download-cloud-2-line'></i> -->
                    <i class='bi bi-download' style='font-size: 2rem;'></i>
                  </span>
                </div>
              </div>
              <div class='col-md-6'>
                <div class='card-preview-block'>
                  <div class='card-canvas-blk' id='canvas'>

                    <p id='canvasText'></p>
                  </div>
                  <div class='form-group'>
                    <label>Receiver Address</label>
                    <input type='text' class='form-control' placeholder='Enter address here...'>
                  </div>
                  <div class='form-btn-blk'>
                    <p>(0.1 SOL + TX FEES)</p>
                    <a href='javascript:void(0);' class='btn mint-btn'>MINT</a>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class='main-footer-blk'>
            <div class='row'>
              <div class='col-md-6'>
                <div class='footer-team-block'>
                  <ul>
                    <li>
                      TEAM
                      <a href='https://twitter.com/kazumidapp' target='_blank'>
                        <figure>
                          <img src='images/team-1.jpg'>
                        </figure>
                        <h6>@kazumidapp</h6>
                      </a>
                    </li>
                    <li>
                      <a href='https://twitter.com/robb747' target='_blank'>
                        <figure>
                          <img src='images/team-2.png'>
                        </figure>
                        <h6>@robb747</h6>
                      </a>
                    </li>
                  </ul>
                </div>
              </div>
              <div class='col-md-6'>
                <div class='footer-content-blk'>
                  <h6 class='good-project'>Funds from this Dapp go to<span class='rainbow rainbow_text_animated'> Kazumi's Good Project DAO</span> to help fund new cool stuff like this!
                      <br>
                      <br>
                    <h6 class='rainbow rainbow_text_animated_slow'>https://goodproject.baby</h6>
                  </h6>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>
    </div>
  </div>
</template>

<script>
import html2canvas from 'html2canvas';

export default {
  data() {
    return {
      bgColors: document.getElementsByClassName('bg-color'),
      txtColors: document.getElementsByClassName('txt-color'),
      canvasText: null,
      canvas: null,
      ctx: null,
      canvasColor: '#eb4d4b',
      txtColor: '#000000',
      currentText: '',
    };
  },
  mounted() {
    this.canvas = document.getElementById('canvas');
    this.canvasText = document.getElementById('canvasText');
  },
  methods: {
    downloadim() {
      console.log('canvas: ', html2canvas);
      const img = document.createElement('a');
      html2canvas(this.canvas, { removeContainer: true }).then((canvas) => {
        const myImage = canvas.toDataURL('image/png');
        img.download = 'postcard.png';
        img.href = myImage;
        img.click();
      });
    },
    colorPick(element) {
      this.canvasColor = element.value;
      this.canvas.style.backgroundColor = this.canvasColor;
    },
    txtColorPick(element) {
      this.txtColor = element.value;
      this.canvasText.style.color = this.txtColor;
    },
    writeText(element) {
      this.currentText = element.value;
      this.canvasText.innerText = this.currentText;
    },
    toggleSelect(element) {
      for (let i = 0; i < this.bgColors.length; i += i) {
        this.bgColors[i].classList.remove('selected');
      }
      element.classList.toggle('selected');
      if (
        element.classList.value === 'bg-color multicolor'
        || element.classList.value === 'bg-color multicolor selected'
      ) {
        console.log('somehting');
      } else {
        this.canvasColor = window
          .getComputedStyle(element)
          .getPropertyValue('background-color');
      }
      this.canvas.style.backgroundColor = this.canvasColor;
    },
    toggleSelectTxtColor(element) {
      for (let i = 0; i < this.txtColors.length; i += i) {
        this.txtColors[i].classList.remove('selected');
      }
      element.classList.toggle('selected');
      this.txtColor = window
        .getComputedStyle(element)
        .getPropertyValue('background-color');
      this.canvasText.style.color = this.txtColor;
    },
  },
};
</script>

<style>
body,
td,
th {
  font-family: 'Lucida Console', Monaco, monospace;
  color: #ffffff;
}
a:link {
  color: #ffffff;
  text-decoration: none;
}
a:visited {
  text-decoration: none;
}
a:hover {
  color: #ffffff;
  text-decoration: underline;
}
a:active {
  text-decoration: none;
}
.rainbow {
  text-align: center;
  letter-spacing: 5px;
}
.rainbow_text_animated {
  background: linear-gradient(
    to right,
    #6666ff,
    #0099ff,
    #00ff00,
    #ff3399,
    #6666ff
  );
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
  animation: rainbow_animation 0.1s ease-in-out infinite;
  background-size: 400% 100%;
}

.rainbow_text_animated_slow {
  background: linear-gradient(
    to right,
    #6666ff,
    #0099ff,
    #00ff00,
    #ff3399,
    #6666ff
  );
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
  animation: rainbow_animation 6s ease-in-out infinite;
  background-size: 400% 100%;
}

@keyframes rainbow_animation {
  0%,
  100% {
    background-position: 0 0;
  }

  50% {
    background-position: 100% 0;
  }
}
</style>
