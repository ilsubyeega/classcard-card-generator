<script>
  let data = [];
  const removeTrailling = (text) => {
    if (text[0] == "'" && text[text.length - 1] == "'")
      text = text.substr(1, text.length - 2);
    return text;
  };

  const openDialog = (isPushable) => {
    const text = window.prompt(
      "데이터를 작성해주세요. 힌트: JSON.stringify(study_data)"
    );
    try {
      const json = JSON.parse(removeTrailling(text));
      if (isPushable) {
        json.forEach((a) => data.push(a));
        data = data;
      } else {
        data = json;
      }
      alert("적용하였습니다.");
    } catch (e) {
      alert(e.message);
    }
  };

  let showNotPracticedOnly = false;
  let boldNotPracticed = true;
</script>

<noprint>
  <button on:click={() => window.print()}>인쇄</button>
  <button on:click={() => openDialog(false)}>데이터 가져오기 (초기화)</button>
  <button on:click={() => openDialog(true)}>데이터 추가하기 (표에 추가)</button>
  <label>
    <input type="checkbox" bind:checked={showNotPracticedOnly} />
    학습하지 않은 단어만 보여주기
  </label>
  <label>
    <input type="checkbox" bind:checked={boldNotPracticed} />
    학습하지 않은 단어 굵은체 처리
  </label>
</noprint>
<!-- {JSON.stringify(data, null, 4)} -->
<div class="center">
  <table>
    <thead>
      <tr>
        <th>단어</th>
        <th>뜻</th>
      </tr>
    </thead>
    <tbody>
      {#each data as word, i}
        {#if !showNotPracticedOnly || word?.known_yn == "-1"}
          <tr
            class={boldNotPracticed && word?.known_yn == "-1" ? "unknown" : ""}
          >
            <td class="word"
              ><p class="index">{i}</p>
              {word.front}</td
            >
            <td class="meaning">{word.back}</td>
          </tr>
        {/if}
      {/each}
    </tbody>
  </table>
</div>

<style>
  @import url("https://fonts.googleapis.com/css2?family=Nanum+Myeongjo:wght@400;700;800&display=swap");
  @media print {
    noprint {
      display: none !important;
    }
    tr {
      break-inside: avoid;
    }
  }
  .unknown {
    font-weight: 600;
  }
  @import url("https://cdn.jsdelivr.net/gh/orioncactus/pretendard/dist/web/static/pretendard.css");

  * {
    font-family: Pretendard, -apple-system, BlinkMacSystemFont, system-ui,
      Roboto, "Helvetica Neue", "Segoe UI", "Apple SD Gothic Neo",
      "Noto Sans KR", "Malgun Gothic", sans-serif;
    font-size: 20px;
  }
  table {
    border-collapse: collapse;
    width: 210mm;
    padding: 10px;
  }
  table,
  td,
  th {
    border: 1px solid;
  }
  td,
  th {
    padding: 5px;
  }
  .index {
    display: inline;
    font-size: 10px;
    margin-right: 4px;
  }
  @page {
    size: A4;
    margin: 0;
  }
  .center {
    display: flex;
    justify-content: center;
  }
  :global(body) {
    margin: 50px;
  }
</style>
