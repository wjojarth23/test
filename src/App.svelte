<script>
import { onMount } from "svelte";
import gsap from "gsap/dist/gsap";
import { ScrollTrigger } from "gsap/dist/ScrollTrigger";
import { ScrollSmoother } from "gsap/dist/ScrollSmoother";

let main, smoother;

  const scrollTo = () => {
    smoother && smoother.scrollTo(".box-c", true, "center center");
  };

  onMount(() => {
    const ctx = gsap.context(() => {
      smoother = ScrollSmoother.create({
        smooth: 2, // seconds it takes to "catch up" to native scroll position
        effects: true, // look for data-speed and data-lag attributes on elements and animate accordingly
      });
      ScrollTrigger.create({
        trigger: ".box-c",
        pin: true,
        start: "center center",
        end: "+=300",
        markers: true,
      });
    }, main); // <- Scope!

    return () => ctx.revert(); // <- Cleanup!
  });
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<div id="smooth-wrapper" bind:this={main}>
  <div id="smooth-content">
    <header class="header">
      <h1 class="title">GSAP ScrollSmoother on a SvelteKit App</h1>
      <button class="button" on:click={scrollTo}> Jump to C </button>
    </header>
    <div class="box box-a" data-speed="0.5">a</div>
    <div class="box box-b" data-speed="0.8">b</div>
    <div class="box box-c" data-speed="1.5">c</div>
    <div class="line" />
  </div>
</div>
<footer>
  <a href="https://greensock.com/scrollsmoother">
    <img
      class="greensock-icon"
      src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/16327/scroll-smoother-logo-light.svg"
      width="220"
      height="70"
      alt="GreenSock"
    />
  </a>
</footer>
