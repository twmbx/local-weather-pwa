<script>
    export let condition;
    export let mini = "";
</script>

<style>
	
</style>

<div class="uk-padding-remove">
        {#if condition == "none"}
        <div class="icon {mini} sun-shower">
            <div class="cloud"></div>
            <div class="sun">
                <div class="rays"></div>
            </div>
            <div class="rain"></div>
        </div>
        {/if}

        {#if condition == "none"}
        <div class="icon {mini} thunder-storm">
            <div class="cloud"></div>
            <div class="lightning">
                <div class="bolt"></div>
                <div class="bolt"></div>
            </div>
        </div>
        {/if}

        {#if condition == "cloudy"}
        <div class="icon {mini} cloudy">
            <div class="cloud"></div>
            <div class="cloud"></div>
        </div>
        {/if}

        {#if condition == "fog"}
        <div class="icon {mini} cloudy">
            <div class="cloud"></div>
            <div class="cloud"></div>
        </div>
        {/if}

        {#if condition == "snow"}
        <div class="icon {mini} flurries">
            <div class="cloud"></div>
            <div class="snow">
                <div class="flake"></div>
                <div class="flake"></div>
            </div>
        </div>
        {/if}

        {#if condition == "sleet"}
        <div class="icon {mini} flurries">
            <div class="cloud"></div>
            <div class="snow">
                <div class="flake"></div>
                <div class="flake"></div>
            </div>
        </div>
        {/if}

        {#if condition == "clear-day"}
        <div class="icon {mini} sunny">
            <div class="sun">
                <div class="rays"></div>
            </div>
        </div>
        {/if}

        {#if condition == "clear-night"}
        <div class="icon {mini} night">
            <div class="moon"></div>
        </div>
        {/if}

        {#if condition == "partly-cloudy-night"}
        <div class="icon {mini} partly-cloudy-night">
            <div class="moon"></div>
            <div class="cloud"></div>
        </div>
        {/if}

        {#if condition == "partly-cloudy-day"}
        <div class="icon {mini} partly-cloudy-day">
            <div class="cloud"></div>
            <div class="sun">
                <div class="rays"></div>
            </div>
        </div>
        {/if}

        {#if condition == "rain"}
        <div class="icon {mini} rainy">
            <div class="cloud"></div>
            <div class="rain"></div>
        </div>
        {/if}
</div>