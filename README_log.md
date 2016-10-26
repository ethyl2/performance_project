## Beginning Page Speed Insights Scores:

__27__ Mobile

__29__ Desktop

## Steps taken to optimize this project:

1. Add `media="print"` to `<link href="css/print.css" rel="stylesheet">`

2. Add `async` to `<script src="http://www.google-analytics.com/analytics.js">`

3. Move the inline script `<script> (function(w,g){w['GoogleAnalyticsObject']=g;` etc. out of the head and into the body.

4. Change style.css content location from `<link href="css/style.css" rel="stylesheet">` to inline `<style>`.

5. Eliminate link to fonts: `<link href="//fonts.googleapis.com/css?family=Open+Sans:400,700" rel="stylesheet">`
 and its corresponding use in the css:
  `body, button, input, select, textarea { font-family: 'Open Sans', sans-serif; color: #333; }`

  #### Effect:

  The steps above increased the scores to __30__.

6. Change source for 3 images from an external link to my images folder. It might
  not make a big change, but it will be handy to have all my images together for
  later changes to improve optimization.
  - `<img src="https://lh4.ggpht.com/kJEnfqhPvtm4m3EneSZ4fWYGS8lW4YNhEjk6zPkyrQaBUHc-2Y_ElDic99NHI0h-UBLXVbRCjFybFvrWxdk=s100">`
  becomes `<img src="img/project2048.jpg">`

  - `<img src="https://lh6.ggpht.com/f_0W8h__3G99CWTjnMjD8BUKm7yp2-wJyApLtTwFoFtlal2ULf_JgHIsOQq2NiYfKOdMlXlMHDKNo5XVZLs=s100">`
  becomes `<img src="img/projectwebperf.jpg">`
  - `<img src="https://lh5.ggpht.com/IKdCmTWn8a2nMhlwMYzryvzRN5CUZAOBr4tDrEAbszV7TIFe9pRAInA4kkYcgTXwrifJsBEsq1agTueuu-g=s100">`
  becomes `<img src="img/projectmobile.jpg">`

7. Minimize `index.html`.

  #### Effect:

  The steps above increased the score to __93__ for mobile and __95__ for desktop.

8. Resize `pizzeria.jpg` and compressed all of the images.

  #### Effect:

  In this case, it didn't
  make a measurable difference, that is, the scores didn't change, but it is
  good practice to resize and compress images.

## Final Page Speed Insights Scores:

  __93__ Mobile

  __95__ Desktop

-------------------------------------------------------------------------------------------------------

## To Improve Performance of Pizza Site:

1. In main.js, in the `changePizzaSizes` function, pull the variables `dx` and `newwidth`
  out of the loop, since their values are the same for all of the generated pizza
  images. Inside, calculate those values once, for `document.querySelectorAll(".randomPizzaContainer")[0]` and then start the loop.

  #### Before the change:

  __95.0 ms__ when sliding from Med -> Small.

  __101.8 ms__ when sliding from Small -> Large.

  #### After the change:

  __5.9 ms__ when sliding from Med -> Small.

  __10.3 ms__ when sliding from Small -> Large.

2. In main.js, in `updatePositions()`, take the following calculation out of the loop.
  Calculate it once instead and assign it a variable.

  `var phaseScroll = document.body.scrollTop / 1250;`

  Then use that variable to calculate the phase variable:

  `var phase = Math.sin(phaseScroll + (i % 5));`

  #### Before the change:

  Ave. scripting time to generate last 10 frames: __24.39 - 29.46 ms__ .

  #### After the change:

  Ave. scripting time to generate last 10 frames: __4.42 ms__ .

3. In `updatePositions()`, use `getElementsByClassName` instead of
  `querySelectorAll` because the former is faster. Here's an article that
  explains why:
  https://www.nczonline.net/blog/2010/09/28/why-is-getelementsbytagname-faster-that-queryselectorall/.
  Basically, the object returned by gEBCN is a Live `NodeList` object, which can
  be created and returned faster by the browser, because they don’t have to have
  all of the information up front. Static `NodeLists` (which result from
  `querySelectorAll`) need to have all of their data from the start.

  `var items = document.querySelectorAll('.mover');`

  becomes

  `var items = document.getElementsByClassName('mover');`

  #### Effect:

  The ave. scripting time to generate last 10 frames decreased to __1.7 - 1.9 ms__ .

4. In `updatePositions()`, pull another calculation out of the loop.
  Because phase calculation is dependent on modulo, all the values returned will be
  only 5 values, between sin(phaseScroll + 0) and sin(phaseScroll + 4).
  Before the loop starts, have the values calculated and inserted into a `phaseArray`
  array.

  ```
  for (var i = 0; i < 5; i++) {
    phaseArray[i] = Math.sin(phaseScroll + i);
  }
  ```

  And then use `phaseArray` inside the main loop:

  `var phase = phaseArray[i % 5];`

  #### Effect:

  The ave. scripting time decreased by about __0.1 ms__ , with a range of __1.3 - 1.6 ms__ .

5. In `document.addEventListener('DOMContentLoaded', function()`, decrease the
  amount of generated animated pizzas from 200 to 50. Even with zooming out the
  page, only 25 pizzas show on the page at a time, so 200 seems unnecessary.
  Even 50 might be more than needed, but should be a good amount, just in case.

  Change the upper limit for `i`:

  `for (var i = 0; i < 50; i++)`

  #### Effect:

  The ave. scripting time decreased to about __0.5 ms__.

6. Back in `changePizzaSizes()`, change all `querySelector` and `querySelectorAll`
  to `getElementById` and `getElementsByClassName`.

  The time differences I measured:

  #### Before the change:

  __12.3 ms__ when sliding from Medium -> Small.

  __8.1 ms__ when sliding from Small -> Large.

  #### After the change:

  __2.8 ms__ when sliding from Medium -> Small.

  __1.7 ms__ when sliding from Small -> Large.

7. Also, in `changePizzaSizes()`, add the variable `containers`, since its value
  is used several times in the function.

  `var containers = document.getElementsByClassName("randomPizzaContainer");`

  #### Effect:

  The times decreased by about __0.1 ms__.

8. Inside the scroll event listener, put the `updatePositions` function inside a
  `requestAnimationFrame()`.

  `window.addEventListener('scroll', function() {
      window.requestAnimationFrame(updatePositions);
  });`

  #### Effect:

  This didn't really make a noticeable effect on the timing, but it is considered
  good coding practice, since `updatePositions()` creates a visible change to the
  page.

9. In `changePizzaSlices`, use the variable 'containers' inside the loop.

10. Take the following line out of its loop, since it would the same every
  iteration of the loop:

  `var pizzasDiv = document.getElementById("randomPizzas");`

  Here is the result:

  ```// This for-loop actually creates and appends all of the pizzas when the page loads
  var pizzasDiv = document.getElementById("randomPizzas");
  for (var i = 2; i < 100; i++) {
    pizzasDiv.appendChild(pizzaElementGenerator(i));
  }
  ```

11. Optimizing that section further, use a document fragment to add the pizzas
  that result from `pizzaElementGenerator` in one chunk instead of one-at-a-time.

  ```var pizzasFragment = document.createDocumentFragment();
  for (var i = 2; i < 100; i++) {
    pizzasFragment.appendChild(pizzaElementGenerator(i));
  }
  pizzasDiv.appendChild(pizzasFragment);
  ```

  The [MDN article](https://developer.mozilla.org/en-US/docs/Web/API/Document/createDocumentFragment)
  explains the advantages of document fragments:
  "Since the document fragment is in memory and not part of the main DOM tree,
  appending children to it does not cause page reflow (computation of element's
  position and geometry). Consequently, using document fragments often results
  in better performance."

  
