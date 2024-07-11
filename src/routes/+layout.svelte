<script>
    //Imports stuff for Tailwind
    import "../app.css";

    //Imports vercel utils
    import { injectSpeedInsights } from '@vercel/speed-insights/sveltekit'
    import { inject } from '@vercel/analytics'

    //Calls vercel utils
    inject()
    injectSpeedInsights()
  </script>
  
  <slot />