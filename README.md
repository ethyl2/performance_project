# Website Performance Optimization Portfolio Project

The challenge for this project was to optimize this online portfolio for speed.
This website had optimization and performance-related issues. By applying the
techniques from the
[Critical Rendering Path course](https://www.udacity.com/course/ud884),
we needed to optimize the critical rendering path and make this page render as
quickly as possible.

### The goals:

1.  Have `index.html` achieve a target `PageSpeed` score of at least 90 for
  Mobile and Desktop.

2. Have `views/main.html` run at least 60 frames per second.

3. Have the time to resize pizzas be less than 5 ms using the pizza size slider
  on the `views/pizza.html` page. Resize time is shown in the browser developer tools.

----------------------------------------------------------------------------------

## Getting started

### Live

Point your browser to https://ethyl2.github.io/performance-project

### Locally

1. Check out the repository:

  ```bash
  $> git clone https://github.com/ethyl2/performance-project
  ````

2. You can run a local server:

  ```bash
  $> cd /path/to/your-project-folder
  $> python -m SimpleHTTPServer 8080
  ```

  Open a browser and visit localhost:8080.

  Alternatively:

  ```bash
  $> open "http://localhost:8080"
  ```

3. Use [ngrok](https://ngrok.com/) to make your local server accessible remotely.

  ``` bash
  $> cd /path/to/your-project-folder
  $> ./ngrok http 8080
  ```

4. Copy the public URL ngrok gives you and try running it through PageSpeed Insights.

---------------------------------------------------------------------------------------------

## Optimization

See [this file](README_log.md) for detailed steps on steps taken to optimize this site.

---------------------------------------------------------------------------------------------

## Resources

FPS Counter/HUD Display in Chrome developer tools: [Chrome Dev Tools tips-and-tricks](https://developer.chrome.com/devtools/docs/tips-and-tricks).

[More on integrating ngrok, Grunt and PageSpeed.](http://www.jamescryer.com/2014/06/12/grunt-pagespeed-and-ngrok-locally-testing/)

#### Optimization Tips and Tricks
* [Optimizing Performance](https://developers.google.com/web/fundamentals/performance/ "web performance")
* [Analyzing the Critical Rendering Path](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/analyzing-crp.html "analyzing crp")
* [Optimizing the Critical Rendering Path](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/optimizing-critical-rendering-path.html "optimize the crp!")
* [Avoiding Rendering Blocking CSS](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/render-blocking-css.html "render blocking css")
* [Optimizing JavaScript](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/adding-interactivity-with-javascript.html "javascript")
* [Measuring with Navigation Timing](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/measure-crp.html "nav timing api").
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/eliminate-downloads.html">The fewer the downloads, the better</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/optimize-encoding-and-transfer.html">Reduce the size of text</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/image-optimization.html">Optimize images</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching.html">HTTP caching</a>

#### Customization with Bootstrap
The portfolio was built on Twitter's <a href="http://getbootstrap.com/">Bootstrap</a> framework. All custom styles are in `dist/css/portfolio.css` in the portfolio repo.

* <a href="http://getbootstrap.com/css/">Bootstrap's CSS Classes</a>
* <a href="http://getbootstrap.com/components/">Bootstrap's Components</a>
