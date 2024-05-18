<script>
    import SplitLetter from "./SplitLetter.svelte";
    import Info from '../../info.json';

    $: isEnglish = true;
    $: lang = isEnglish ? 'en' : 'et';
    $: data = Info[lang];

    function slideIn(node, { dir = "left" }) {
        node.style.transform =
            dir == "left" ? "translateX(-100%)" : "translateX(100%)";
        node.style.transition = "all 0.4s";
        let once = false;
        return {
            duration: 300,
            tick: (t) => {
                if (!once && t > 0) {
                    node.style.transform = "translateX(0)";
                    once = true;
                }
            },
        };
    }
</script>
<div id="background" class="two-columns">
    <div>
        <svg xmlns="http://www.w3.org/2000/svg" style="opacity: 30%;" version="1.1" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:svgjs="http://svgjs.dev/svgjs" viewBox="0 0 700 700" width="700" height="700"><defs><filter id="nnnoise-filter" x="-20%" y="-20%" width="140%" height="140%" filterUnits="objectBoundingBox" primitiveUnits="userSpaceOnUse" color-interpolation-filters="linearRGB">
            <feTurbulence type="fractalNoise" baseFrequency="0.112" numOctaves="4" seed="15" stitchTiles="stitch" x="0%" y="0%" width="100%" height="100%" result="turbulence"></feTurbulence>
            <feSpecularLighting surfaceScale="12" specularConstant="0.75" specularExponent="20" lighting-color="#a87457" x="0%" y="0%" width="100%" height="100%" in="turbulence" result="specularLighting">
                    <feDistantLight azimuth="3" elevation="117"></feDistantLight>
              </feSpecularLighting>
          
        </filter></defs><rect width="700" height="700" fill="transparent"></rect><rect width="700" height="700" fill="#a87457" filter="url(#nnnoise-filter)"></rect></svg>
    </div>
    <div id="right">
        <svg xmlns="http://www.w3.org/2000/svg" version="1.1" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:svgjs="http://svgjs.dev/svgjs" viewBox="0 0 700 700" width="700" height="700"><defs><filter id="nnnoise-filter" x="-20%" y="-20%" width="140%" height="140%" filterUnits="objectBoundingBox" primitiveUnits="userSpaceOnUse" color-interpolation-filters="linearRGB">
            <feTurbulence type="fractalNoise" baseFrequency="0.112" numOctaves="4" seed="15" stitchTiles="stitch" x="0%" y="0%" width="100%" height="100%" result="turbulence"></feTurbulence>
            <feSpecularLighting surfaceScale="12" specularConstant="0.75" specularExponent="20" lighting-color="#a87457" x="0%" y="0%" width="100%" height="100%" in="turbulence" result="specularLighting">
                    <feDistantLight azimuth="3" elevation="117"></feDistantLight>
              </feSpecularLighting>
          
        </filter></defs><rect width="700" height="700" fill="transparent"></rect><rect width="700" height="700" fill="#a87457" filter="url(#nnnoise-filter)"></rect></svg>
    </div>
</div>

<div id="container" class="two-columns">
    <section in:slideIn>
        <div class="scroll-fix info" style="margin-left: auto; width: fit-content;">
            <!-- <h1>Akadeemia Kafe</h1> -->
            <img src="./logo.png" />
            <div class="hours">
                <h2>{data['hours']['title']}</h2>
                <ul>    
                    {#each data['hours']['hours'] as period}
                        <li><h3>{period}</h3></li>    
                    {/each}
                </ul>
            </div>
            <iframe width="100%" height="450" style="border:0" loading="lazy" title="Map with the cafe pointed out"
                src="https://www.google.com/maps/embed/v1/place?q=place_id:Eh9Ba2FkZWVtaWEgdGVlLCBUYWxsaW5uLCBFc3RvbmlhIi4qLAoUChIJnRFu5ayVkkYRhst7JPA25HcSFAoSCb8WVt-ZlJJGEXBi_BhtswAE&key=AIzaSyDI3i-iv6QZ3LAALyxZxkY_t4vLZLCXA9I"></iframe> 
        </div>
        <!-- <div>test</div> -->
    </section>
    <section in:slideIn={{ dir: "right" }} class="col board">
        <div class="lang">
            <span class:lang-active={isEnglish}><a href="#" on:click={()=>{isEnglish = true}}><h3>EN</h3></a></span>
            <span class:lang-active={!isEnglish}><a href="#" on:click={()=>{isEnglish = false}}><h3>ET</h3></a></span>
        </div>
        <div class="scroll-fix board-filter">
            <div class="description">
                <div class="entry" style="--gradient-gray: 10%; --gradient-black: 70%;"><h1><SplitLetter word={data['description']['title']} className="board-letter" /></h1></div>
                {#each data['description']['text'] as textEntry}
                    <p>
                        {textEntry}
                    </p>    
                {/each}
            </div>
            {#each data['menu'] as menuSection}
                <div class="menu-section">
                    <div class="entry" style="--gradient-gray: 25%; --gradient-black: 80%;"><h1><SplitLetter word={menuSection['title']} className="board-letter" /></h1></div>
                    {#each menuSection['items'] as menuItem} 
                        <div class="entry" style="--gradient-gray: ${menuItem[2] || '45%'}; --gradient-black: ${menuItem[3] || '80%'};">
                            <h2><SplitLetter word={menuItem[0]} className="board-letter" /></h2>
                            <h2><SplitLetter word={menuItem[1]} className="board-letter" /></h2>
                        </div> 
                    {/each}
                </div>
            {/each}
        </div>
    </section>
</div>

<style lang="scss">
    #container {
        position: relative;
    }
    .board {
        @media screen and (max-width: 1250px) {
            display: block;
            background-color: #d7bfa8;
            background: linear-gradient(180deg, rgba(162,146,131,1) 0%, rgba(179,155,133,1) 35%, rgba(186,154,123,1) 79%);  
        }
    }

    .board-filter {
        backdrop-filter: blur(1px);
    }
    

    .two-columns {
        height: 100vh;
        width: 100%;
        overflow-y: auto;
        display: flex;
        flex-direction: row;
        justify-content: space-evenly;
        align-items: start;

        @media screen and (max-width: 1250px) {
            display: block;
        }
    }

    #background {
        position: absolute;
        top:0;
        left: 0;
        overflow-y: auto;
    }

    #background div {
        width: 100%;
        align-self: stretch;
    }
    #background #right {
        box-shadow: 4px 8px 16px rgba(0, 0, 0, 0.4), 5px 5px 10px rgba(0, 0, 0, 0.3);
        background-color: #d7bfa8;
        background: linear-gradient(180deg, rgba(162,146,131,1) 0%, rgba(179,155,133,1) 35%, rgba(186,154,123,1) 79%);  
        position: relative;
    }
    #background div svg {
        opacity: 50%;
        position: absolute;
        width: 100%;
        height: 100%;
        left: 0;
        top: 0;
        /* height: 100%; */
    }
    .scroll-fix {
        max-width: 80em;
        /* margin: auto; */
        @media screen and (max-width: 1250px) {
            margin: auto;
        }
    }
    section {
        box-sizing: border-box;
        font-family: Arial, Helvetica, sans-serif;
        padding: 2em;
        /* margin-top: auto; */
        flex-basis: 50%;
        position: -webkit-sticky;
        position: sticky;
        top: -20vh;
        
        @media screen and (max-width: 1250px) {
            position: relative;
        }

        @media screen and (max-width: 1250px) and (orientation: portrait){
            /* position: relative; */
            position: sticky;
            top: -70vh;
        }

        @media screen and (max-width: 640px) {
            padding: 2em 0;
        }
    }
    h2 {
        font-size: 2rem;

        @media screen and (max-width: 640px) {
            font-size: 1.5rem;
        }
    }
    h1 {
        font-size: 3rem;
        
        @media screen and (max-width: 640px) {
            font-size: 2rem;
        }
    }

    /* Section for selecting language */
    .lang {
        display: flex;
        direction: col;
        gap: 20px;
    }
    .lang a {
        text-decoration: none;   
    }
    .lang a {
        color: black;
    }
    .lang-active a {
        color: white;
    }
    /* Left section */
    .info {
        text-align: center;
        margin: auto;
    }

    .info img {
        display: block;
        width: 80%;
        margin: auto;
        padding: 20px 0;
        max-width: 500px;
        min-width: 300px;
    }

    .info iframe {
        margin-top: 3em;
    }
    /* Hours of operation */
    .hours ul {
        font-family: "JetBrains Mono", monospace;
        list-style: none;
        font-size: 1.3em;
        position: relative;
        padding: 0;
        width: fit-content;
        margin: auto;
        padding: 0 25px;
        @media screen and (max-width: 640px) {
            font-size: 1.1rem;
        }
    }

    .hours ul::before {
        position: absolute;
        left: 0;
        top: 0;
        border: 2px solid black;
        border-right: 0;
        width: 10px;
        height: 100%;
        content: "";
    }
    .hours ul::after {
        position: absolute;
        right: 0;
        top: 0;
        border: 2px solid black;
        border-left: 0;
        width: 10px;
        height: 100%;
        content: "";
    }
    .menu-section {
        padding-bottom: 2em;
    }

    .entry :global(.board-letter) {
      border-right: 1px solid rgba(0, 0, 0, 0.2);
      padding: 0 1px;
    }

    .entry {
        margin-top: 1em;
        width: 100%;
        position: relative;
        font-family: "JetBrains Mono", monospace;
        font-optical-sizing: auto;
        font-weight: 400;
        font-style: normal;
        display: flex;
        flex-direction: row;
        justify-content: space-between;
    }
    .entry::before {
        z-index: 1;
        content: "";
        width: 100%;
        position: absolute;
        top: 50%;
        left: 0;
        height: 4px;
        background: rgb(224,224,224);
        background: linear-gradient(90deg, rgba(224,224,224,1) 0%, rgba(255,255,255,1) var(--gradient-gray, 35%), rgba(0,0,0,1) var(--gradient-black, 79%));
    }
    .entry * {
        box-shadow: 2px 4px 8px rgba(0, 0, 0, 0.3), 5px 5px 10px rgba(0, 0, 0, 0.2);
        color: #222;
        margin: 0 25px;
        position: relative;
        z-index: 2;
        text-transform: uppercase;
        background-color: rgb(250, 246, 235);
        line-height: 130%;
        width: fit-content;

        
        @media screen and (max-width: 640px) {
            margin: 0 25px 0 0;
        }
    }

    .description {
        padding: 10px 0;
        background-color: rgb(250, 246, 235);
        margin-bottom: 2em;
        box-shadow: 2px 4px 8px rgba(0, 0, 0, 0.3), 5px 5px 10px rgba(0, 0, 0, 0.2);
        /* backdrop-filter: brightness(60%); */
    }
    .description p {
        padding: 10px;
    }

    
    /* Right side */
    .board p {
        padding-left: 20px;
        padding-right: 20px;
        font-size: 1.8rem;
    }

    



    .left {
        transition: transform 1s;
        transform: translateX(-100%);
    }

    .left-visible {
        transform: translateX(0);
    }
</style>
