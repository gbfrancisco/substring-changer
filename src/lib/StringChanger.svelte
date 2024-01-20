<script>
  let text;
  let lines;

  // inputs
  let line;
  let start;
  let end;
  let newStr;

  let isInvalid = true;

  const getStringLines = str => str.split(/\r?\n/);

  $: changeString = () => {
    const lineToModify = lines[+line - 1];
    const firstPart = lineToModify.substring(0, +start - 1);
    const lastPart = lineToModify.substring(+end);
    lines[+line - 1] = firstPart + newStr + lastPart;
    text = lines.join('\r\n');
  }

  $: areInputsInvalid = () => {
    if (!text || !line || !start || !end || !newStr) {
      return true;
    }

    if (line < 1 || start < 1 || end < 1) {
      return true;
    }

    if (start > end) {
      return true;
    }

    if (line > lines.length) {
      return true;
    }

    const startEndLength = +end - +start + 1;
    return newStr.length !== startEndLength || newStr.length > text.length || end > lines[+line - 1].length;
  }

  $: isInvalid = areInputsInvalid();

  $: if (text) {
    lines = getStringLines(text);
  }
</script>

<div>
  <textarea class="text-area-1" bind:value={text} />
  <div class="container main-container">
    <div class="container line-input-container">
      <span class="label">Line:</span>
      <input class="line-input" type="number" bind:value={line} />
    </div>
    <div class="container start-position-input-container">
      <span class="label">Start:</span>
      <input class="start-position-input" type="number" bind:value={start} />
    </div>
    <div class="container end-position-input-container">
      <span class="label">End:</span>
      <input class="end-position-input" type="number" bind:value={end} />
    </div>
    <div class="container new-string-input-container">
      <span class="label">New String:</span>
      <input class="new-string-input" bind:value={newStr} />
    </div>
  </div>
  <div class="container button-group-container">
    <button class="change-string-button" disabled={isInvalid} on:click={changeString}>
      Change String
    </button>
  </div>
</div>

<style>
  /* Chrome, Safari, Edge, Opera */
  input::-webkit-outer-spin-button,
  input::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }

  /* Firefox */
  input[type=number] {
    -moz-appearance: textfield;
    appearance: textfield;
  }

  textarea {
    min-height: 300px;
    max-height: 500px;
    min-width: 750px;
    max-width: 1000px;
    white-space: nowrap;
    resize: both;
    font-size: 1rem;
  }

  input {
    font-size: 1rem;
    height: 1.75rem;
  }

  .container {
    display: flex;
    align-items: center;
  }

  .main-container .container:not(:last-child) {
    margin-right: 10px;
  }

  .button-group-container {
    display: flex;
    justify-content: center;
  }

  .main-container {
    margin: 10px 0;
  }

  .new-string-input-container {
    width: 100%;
  }

  .label {
    font-weight: bold;
    margin-right: 10px;
  }

  .line-input {
    width: 2.5rem;
  }

  .start-position-input {
    width: 4rem;
  }

  .end-position-input {
    width: 4rem;
  }

  .new-string-input {
    flex: 1;
  }

  .change-string-button {
    margin-top: 10px;
    background-color: #f16336;
    color: white;
  }

  .change-string-button:active {
    background-color: #f18436;
  }

  .change-string-button:disabled {
    border: none;
    background-color: #cccccc;
    color: #666666;
  }
</style>