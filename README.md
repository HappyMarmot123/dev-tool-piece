FPS Frames rendered in the last second. The higher the number the better.      
MS Milliseconds needed to render a frame. The lower the number the better.      
MB MBytes of allocated memory. (Run Chrome with --enable-precise-memory-info)      
CUSTOM User-defined panel support.      

javascript:(function(){var script=document.createElement('script');script.onload=function(){var stats=new Stats();document.body.appendChild(stats.dom);requestAnimationFrame(function loop(){stats.update();requestAnimationFrame(loop)});};script.src='https://mrdoob.github.io/stats.js/build/stats.min.js';document.head.appendChild(script);})()

Translate

new class e{constructor(){this.init()}init(){this.handleMouseUp=this.handleMouseUp.bind(this),this.handleKeyDown=this.handleKeyDown.bind(this),this.handleSelectionChange=this.handleSelectionChange.bind(this),document.addEventListener("mouseup",this.handleMouseUp),document.addEventListener("keydown",this.handleKeyDown),document.addEventListener("selectionchange",this.handleSelectionChange),this.initializeStyles()}initializeStyles(){let e=document.createElement("style");e.textContent="@keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }",document.head.appendChild(e)}handleMouseUp(e){if(e.target.closest("#selection-buttons"))return;this.removeExistingLayer();let t=window.getSelection().toString();if(!t)return;let n=window.getSelection().getRangeAt(0),i=n.getBoundingClientRect();localStorage.getItem("OPENAI_API_KEY"),this.createButtonContainer(i)}createButtonContainer(e){let t=document.createElement("div");t.id="selection-buttons",t.style.position="fixed",t.style.left=`${e.left}px`,t.style.top=`${e.bottom+5}px`,t.style.zIndex="999999",t.style.background="white",t.style.border="1px solid #ccc",t.style.padding="5px",t.style.borderRadius="4px",t.style.boxShadow="0 2px 5px rgba(0,0,0,0.2)",this.createButtons(t),document.body.appendChild(t)}createButtons(e){["요약","번역","스피치","APIKEY설정"].forEach(t=>{let n=document.createElement("button");n.textContent=t,n.style.marginRight="5px",n.addEventListener("click",e=>this.handleButtonClick(e,t)),e.appendChild(n)})}async handleButtonClick(e,t){if(e.stopPropagation(),"APIKEY설정"===t){this.handleApiKeySettings();return}if(!localStorage.getItem("OPENAI_API_KEY")){alert("API Key를 먼저 설정해주세요.");return}let n=window.getSelection().toString();if(!n){alert("선택된 텍스트가 없습니다.");return}let i=this.createSpinner();try{switch(t){case"요약":await this.handleSummarize(n,i);break;case"번역":await this.handleTranslate(n,i);break;case"스피치":await this.handleSpeech(n,i)}}catch(s){console.error(`${t} 처리 중 오류:`,s),i.remove()}}handleApiKeySettings(){let e=prompt("OpenAI API Key를 입력하세요:");e&&(localStorage.setItem("OPENAI_API_KEY",e),alert("API Key가 저장되었습니다."))}createSpinner(){let e=document.createElement("div");e.id="loading-spinner-container",e.style.position="fixed",e.style.top="0",e.style.left="0",e.style.width="100%",e.style.height="100%",e.style.background="rgba(0, 0, 0, 0.5)",e.style.display="flex",e.style.justifyContent="center",e.style.alignItems="center",e.style.zIndex="9999999";let t=document.createElement("div");return t.style.width="50px",t.style.height="50px",t.style.border="5px solid #f3f3f3",t.style.borderTop="5px solid #3498db",t.style.borderRadius="50%",t.style.animation="spin 1s linear infinite",e.appendChild(t),document.body.appendChild(e),e}async handleSummarize(e,t){let n=await this.callOpenAI(e,"주어진 텍스트를 절반 정도의 길이로 핵심 내용만 요약해주세요.");this.updateSelectedText(n,t)}async handleTranslate(e,t){let n=await this.callOpenAI(e,"당신은 전문 번역가입니다. 주어진 텍스트를 한국어로 자연스럽게 번역해주세요.");this.updateSelectedText(n,t)}async handleSpeech(e,t){let n=await fetch("https://api.openai.com/v1/audio/speech",{method:"POST",headers:{Authorization:`Bearer ${localStorage.getItem("OPENAI_API_KEY")}`,"Content-Type":"application/json"},body:JSON.stringify({model:"tts-1",input:e,voice:"alloy"})}),i=await n.blob(),s=new Audio(URL.createObjectURL(i));s.play(),t.remove()}async callOpenAI(e,t){let n=await fetch("https://api.openai.com/v1/chat/completions",{method:"POST",headers:{Authorization:`Bearer ${localStorage.getItem("OPENAI_API_KEY")}`,"Content-Type":"application/json"},body:JSON.stringify({model:"gpt-4o-mini",messages:[{role:"system",content:t},{role:"user",content:e}]})}),i=await n.json();return i.choices[0].message.content}updateSelectedText(e,t){let n=window.getSelection(),i=n.getRangeAt(0);i.deleteContents(),i.insertNode(document.createTextNode(e)),t.remove();let s=window.getSelection().getRangeAt(0),a=s.getBoundingClientRect(),l=document.getElementById("selection-buttons");l&&(l.style.left=`${a.left}px`,l.style.top=`${a.bottom+5}px`)}handleKeyDown(e){"Escape"===e.key&&this.removeExistingLayer()}handleSelectionChange(){window.getSelection().toString()||this.removeExistingLayer()}removeExistingLayer(){let e=document.getElementById("selection-buttons");e&&e.remove()}destroy(){document.removeEventListener("mouseup",this.handleMouseUp),document.removeEventListener("keydown",this.handleKeyDown),document.removeEventListener("selectionchange",this.handleSelectionChange),this.removeExistingLayer()}};

React HOC

function LoginStartPage() {
  /* ... 로그인 관련 로직 ... */
  return <>{/* ... 로그인 관련 컴포넌트 ... */}</>;
}

export default withAuthGuard(LoginStartPage);

// HOC 정의
/* ... AuthGuard Paramter는 WrappedComonent Argument Props 이다 ... */
function withAuthGuard(WrappedComponent) {
  return function AuthGuard(props) {
    const status = useCheckLoginStatus();

    useEffect(() => {
      if (status === "LOGGED_IN") {
        location.href = "/home";
      }
    }, [status]);

    return status !== "LOGGED_IN" ? <WrappedComponent {...props} /> : null;
  };
}
