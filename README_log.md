## Beginning Page Speed Insights Scores:

27 Mobile

29 Desktop

## Steps taken to optimize this project:

1. Add `media="print"` to `<link href="css/print.css" rel="stylesheet">`

2. Add `async` to `<script src="http://www.google-analytics.com/analytics.js">`

3. Move the inline script `<script> (function(w,g){w['GoogleAnalyticsObject']=g;` etc. out of the head and into the body.

4. Change style.css content location from `<link href="css/style.css" rel="stylesheet">` to inline `<style>`.

5. Eliminate link to fonts: `<link href="//fonts.googleapis.com/css?family=Open+Sans:400,700" rel="stylesheet">`
 and its corresponding use in the css:
  `body, button, input, select, textarea { font-family: 'Open Sans', sans-serif; color: #333; }`

  The steps above increased the scores to 30.

6. Change source for 3 images from an external link to my images folder. It might
  not make a big change, but it will be handy to have all my images together for
  later changes to improve optimization.
  - `<img src="https://lh4.ggpht.com/kJEnfqhPvtm4m3EneSZ4fWYGS8lW4YNhEjk6zPkyrQaBUHc-2Y_ElDic99NHI0h-UBLXVbRCjFybFvrWxdk=s100">`
  becomes `<img src="img/project2048.jpg">`

  - `<img src="https://lh6.ggpht.com/f_0W8h__3G99CWTjnMjD8BUKm7yp2-wJyApLtTwFoFtlal2ULf_JgHIsOQq2NiYfKOdMlXlMHDKNo5XVZLs=s100">`
  becomes `<img src="img/projectwebperf.jpg">`
  - `<img src="https://lh5.ggpht.com/IKdCmTWn8a2nMhlwMYzryvzRN5CUZAOBr4tDrEAbszV7TIFe9pRAInA4kkYcgTXwrifJsBEsq1agTueuu-g=s100">`
  becomes `<img src="img/projectmobile.jpg">`

7. Minimize index.html

  The steps above increased the score to 93 for mobile and 95 for desktop.

8. Resize pizzeria.jpg and compressed all of the images. Didn't make a big difference.

-------------------------------------------------------------------------------------------------------

## To Improve Performance of Pizza Site:

1. In main.js, in the `changePizzaSizes` function, pull the variables `dx` and `newwidth`
  out of the loop, since their values are the same for all of the generated pizza
  images. Inside, calculate those values once, for `document.querySelectorAll(".randomPizzaContainer")[0]` and then start the loop.

  ### Before the change:

  95.0 ms when sliding from Med -> Small.

  101.8 ms when sliding from Small -> Large.

  ### After the change:

  5.9 ms when sliding from Med -> Small.

  10.3 ms when sliding from Small -> Large.

2. In main.js, in `updatePositions()`, take the following calculation out of the loop.
  Calculate it once instead and assign it a variable.

  `var phaseScroll = document.body.scrollTop / 1250;`

  Then use that variable to calculate the phase variable:

  `var phase = Math.sin(phaseScroll + (i % 5));`

  ### Before the change:

  Ave. scripting time to generate last 10 frames: 24.39 - 29.46 ms.

  ### After the change:

  Ave. scripting time to generate last 10 frames: 4.42 ms.

3. In `updatePositions()`, use `getElementsByClassName` instead of `querySelectorAll`
  because the former is faster. Here's an article that explains why: https://www.nczonline.net/blog/2010/09/28/why-is-getelementsbytagname-faster-that-queryselectorall/.
  Basically, the object returned by gEBCN is a Live `NodeList` object, which can be created and returned faster by the browser, because they don’t have to have all of the information up front. Static `NodeLists` (which result from `querySelectorAll`) need to have all of their data from the start.

  `var items = document.querySelectorAll('.mover');`

  becomes

  `var items = document.getElementsByClassName('mover');`

  The ave. scripting time to generate last 10 frames decreased to 1.7 - 1.9 ms.

4. In `updatePositions()`, pull another calculation out of the loop.
  Because phase calculation is dependent on modulo, all the values returned will be
  only 5 values, between sin(phaseScroll + 0) and sin(phaseScroll + 4).
  Before the loop starts, have the values calculated and inserted into a `phaseArray`
  array.

  ```for (var i = 0; i < 5; i++) {
    phaseArray[i] = Math.sin(phaseScroll + i);
  }
  ```
  And then use `phaseArray` inside the main loop:

  `var phase = phaseArray[i % 5];`

  The ave. scripting time decreased by about 0.1 ms, with a range of 1.3 - 1.6 ms.
