<script>
    import { onMount } from 'svelte';
  
    onMount(() => {
      const script = document.createElement('script');
      script.src = "https://cdn.jsdelivr.net/npm/particles.js@2.0.0/particles.min.js";
      script.onload = () => {
        particlesJS('particles-js', {
          "particles": {
            "number": {
              "value": 50,
              "density": {
                "enable": true,
                "value_area": 200
              }
            },
            "color": {
              "value": "#ffdd57"
            },
            "shape": {
              "type": "circle",
              "stroke": {
                "width": 0,
                "color": "#000000"
              }
            },
            "opacity": {
              "value": 1,
              "random": true,
              "anim": {
                "enable": true,
                "speed": 1,
                "opacity_min": 0,
                "sync": false
              }
            },
            "size": {
              "value": 3,
              "random": true,
              "anim": {
                "enable": true,
                "speed": 10,
                "size_min": 0.1,
                "sync": false
              }
            },
            "line_linked": {
              "enable": false
            },
            "move": {
              "enable": true,
              "speed": 5,
              "direction": "none",
              "random": true,
              "straight": false,
              "out_mode": "out",
              "bounce": false,
              "attract": {
                "enable": false,
                "rotateX": 600,
                "rotateY": 600
              }
            }
          },
          "interactivity": {
            "detect_on": "canvas",
            "events": {
              "onhover": {
                "enable": false
              },
              "onclick": {
                "enable": false
              },
              "resize": true
            }
          },
          "retina_detect": true
        });
      };
      document.body.appendChild(script);
    });
  </script>
  
  <style>
    .spark-container {
      pointer-events: none; /* Ensure the container doesn't interfere with user interaction */
    }
  </style>
  
  <div class="wire w-full h-1 bg-gray-600 relative mx-auto my-16">
    <div id="particles-js" class="spark-container absolute top-[-20px] left-1/2 transform -translate-x-1/2 w-24 h-24"></div>
  </div>
  