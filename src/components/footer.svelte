<script lang="ts">

import { onMount } from "svelte";
import { clickables } from "../store";

let blogLink, linkedInLink, twitterLink

onMount(() => {
	clickables.update(value => [...value, blogLink, linkedInLink, twitterLink]);
})

</script>

<style lang="sass">

@import "../consts.sass"
@include textStyles()

.footer-wrapper
    width: 100vw
    background-color: #131314
    display: flex
    flex-direction: row
    justify-content: space-between
    padding: 15vh 13vw
    margin-top: 25vh
    box-sizing: border-box

    @media only screen and (max-width: 950px)
        flex-direction: column-reverse

        .flex-wrapper:not(:first-child)
            margin-bottom: 15vh


    .inline-flex
        flex-grow: 1
        display: flex
        flex-direction: row
        align-items: center

        .logo
            display: inline-block
            height: 6vh
        p.year
            font-family: $font
            font-size: 3.5vh
            font-weight: normal
            margin-left: 0.7vw

    .button-wrapper
        margin-top: 5vh
        margin-bottom: 3vh

        .button
            font-size: 3.5vh
            margin-right: 1vw
    
    .credits
        font-size: 1.5vh
        white-space: nowrap

    .large
        font-size: 2.5vh

        @media only screen and (max-width: 950px)
            br
                display: none

</style>

<div class="footer-wrapper">
	<div class="flex-wrapper">
        <div class="inline-flex">
            <img src="assets/imgs/footer-logo.svg" alt="vk logo" class="logo">
            <p class="year">© 2023</p>
        </div>
        <div class="button-wrapper">
            <span class="button" bind:this={twitterLink}><a href="https://twitter.com/3lvis_vk" target="_blank" class="clickable sublink link">Twitter</a></span>
		    <span class="button" bind:this={linkedInLink}><a href="https://www.linkedin.com/in/vineet-kumar-a67792197/" target="_blank" class="clickable sublink link">LinkedIn</a></span>
		    <span class="button" bind:this={blogLink}><a href="https://vk-blog.netlify.app/" target="_blank" class="clickable sublink link">Blog</a></span>
        </div>
        <p class="credits">
            Create with the heart;<br>
            build with the mind.
        </p>
    </div>
    <div class="flex-wrapper">
        <p class="large">
            i am currently accepting freelance work,<br> you may reach me on my email.
        </p><br><br>
        <a class="button large" href="mailto:vk.ind004@gmail.com" target="_blank">vk.ind004@gmail.com</a>
    </div>
</div>