# Intro
This document is 

## Toolbox
### Input value
We are going to use this slider to control the values 
```html
<input type="range" id="someInputID" min="0" max="100" step="0.5" value="50" />
```

### Output value
This component will print the value of the input with the `id` matchig his `for` attribute.
```html
<output id="someOutputID" for="someInputID"></output>
```

### Form structure

```html
<form oninput="someOutputID.value=someInputID.value">
  <input type="range" id="someInputID" min="0" max="50" step="0.5" value="0.5" />
  <output id="someOutputID" for="someInputID"></output>
</form>
```

### Button
```html
<button onclick="some.action();">DESCRIPTIVE LABEL</button>
```

## Walkthrought
### 1. Get audio context 
[AudioContext in details, pt - 1](https://developer.mozilla.org/en-US/docs/Web/API/AudioContext)
[AudioContext in details, pt - 2](https://developer.mozilla.org/en-US/docs/Web/API/BaseAudioContext)
```javascript
const audio_context = window.AudioContext || window.webkitAudioContext;
const con = new audio_context();
```

### 2. Define the oscilaltor

```javascript
const osc = con.createOscillator();
const lfo = con.createOscillator();

// here we are preparing an effect
const lfoAmp = con.createGain();
```

### 3. Linking frequency and value
```javascript
osc.frequency.value = SomeInputID.value;
lfo.frequency.value = 300;
```




## Final code 
### HTML 
```html
<form oninput="oscN.value=parseInt(oscFreq.value)">
<h2>Oscilaltor frequency</h2>
  <input type="range" id="oscFreq" min="0" max="500" step="1" value="300"/>
  <output id="oscN" for="oscFreq"></output>
</form>

<form oninput="lfoN.value=lfoFreq.value">
<h2>Low oscillator frequency</h2>
  <input type="range" id="lfoFreq" min="0" max="50" step="0.5" value="0.5" />
  <output id="lfoN" for="lfoFreq"></output>
</form>

<form oninput="gainN.value=parseInt(gainVal.value)">
<h2>Gain value</h2>
  <input type="range" id="gainVal" min="100" max="500" step="10" value="200"/>
  <output id="gainN" for="gainVal"></output>
</form>

<button onclick="osc.start();">start</button>
<button onclick="osc.stop();">stop</button>
```

### JAVASCRIPT
```javascript
// Crossbrowsing
const audio_context = window.AudioContext || window.webkitAudioContext;

// Creating the oscillator
const con = new audio_context();
const osc = con.createOscillator();
const lfo = con.createOscillator();
const lfoAmp = con.createGain();

// Grabbing values
const oscFreq = document.getElementById('oscFreq');
const oscN = document.getElementById('oscN');
const lfoFreq = document.getElementById('lfoFreq');
const lfoN = document.getElementById('lfoN');
const gainVal = document.getElementById('gainVal');
const gainN = document.getElementById('gainN');

// Printing value in par.
oscN.value = parseInt(oscFreq.value);
lfoN.value = lfoFreq.value;
gainN.value = parseInt(gainVal.value);

//Define frequency 
osc.frequency.value = oscFreq.value;
lfo.frequency.value = lfoFreq.value;

// Define gain. 
lfoAmp.gain.value = gainVal.value;

// Chenges.
oscFreq.addEventListener('input', function() {
  osc.frequency.value = oscFreq.value;
});
lfoFreq.addEventListener('input', function() {
  lfo.frequency.value = lfoFreq.value;
});
gainVal.addEventListener('input', function() {
  lfoAmp.gain.value = gainVal.value;
});

// Define destination as computer standard. 
lfo.connect(lfoAmp);
lfoAmp.connect(osc.frequency);
osc.connect(con.destination);

lfo.start();
```
