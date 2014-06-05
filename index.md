---
layout: page
title: Interface Vision - Home
id: home
tagline: Empowering Domain Experts 
description: Interface Vision is creating awesome tools which enable domain experts to program.
theme:
  name: twitter
---
{% include JB/setup %}

<div id="myCarousel" class="carousel slide">
  <div class="carousel-inner">
    <div class="item active">
      <img src="{{ ASSET_PATH }}/img/carousel/slide-05.jpg" alt="">
      <div class="container">
        <div class="carousel-caption">
          <h1>Empowering Domain Experts</h1>
          <p class="lead">Through a simplified programming experience</p>
          <a class="btn btn-large btn-success" href="./signup.html">Get involved</a>
        </div>
      </div>
    </div>
  </div>
</div>

<div class="container marketing">
  
  <div class="featurette">
    <h2 class="featurette-heading">Our Core Technology</h2>
    <p class="lead">We're empowering domain experts through the SipCoffee Language and <b><i>VISION</i></b>.</p>
    <hr class="featurette-divider">
    <h2 class="featurette-heading">The SipCoffee Language <span class="muted">simple and powerful</span></h2>
    <p class="lead"><b><i><a href="http://blog.interfacevision.com/design/design-composition-based-language/">SipCoffee</a></i></b> is a programming language with very minimal syntax. Programming in <b><i><a href="http://blog.interfacevision.com/design/design-composition-based-language/">SipCoffee</a></i></b> is done through the composition of messages. In SipCoffee, even constructs like for loops, conditional statements, variables and scope are messages.</p>
    <p>
      <pre>
        <code>
          // Application, WithEach and WriteLine are messages
          // do, item and text are properties
          Application (
            do WithEach (
              item 1 2 4 8 16 32
              do WriteLine ( text CurrentItem () )
            )
          )
        </code>
      </pre>
    </p>
    <hr class="featurette-divider">
    <img class="featurette-image pull-right" src="{{ ASSET_PATH }}/img/index/iPadMockupVision524x409.png">
    <h2 class="featurette-heading"><b><i>VISION</i> </b><span class="muted">A development environment for domain experts</span></h2>
    <p class="lead"><b><i>VISION</i></b> is a visual programming environment designed for domain experts. An unintended, but awesome outcome, of our composition centric language is that it is really easy to use in a visual programming environment.</p>
    <hr class="featurette-divider">
    <h2 class="featurette-heading">Design Goals and Features</h2>
    <p class="lead">SipCoffee and Vision are designed with community, consistency, collaboration, learnability, sharing, scalability and concurrency, assured backwards compatibility, immediate feedback, regression testing, separation of mechanisms from business and multiple mental-models in mind.</p>
    <p class="lead"><span class="muted">Community</span> - Most important is a solution that can build a strong following.</p>
    <p class="lead"><span class="muted">Consistency</span> - The syntax to describe variables, loops and conditions is the same.</p>
    <p class="lead"><span class="muted">Collaboration</span> - Developers, designers, product owners, entrepreneurs and their customers can work together in a single development environment.</p>
    <p class="lead"><span class="muted">Learnability</span> - We remove as many abstract programming concepts as possible (functions, methods, inheritance, closures, etc.) leaving a minimal set of concepts for domain experts to learn.</p>
    <p class="lead"><span class="muted">Sharing</span> - Programs and their parts should be very easy to re-use and share.</p>
    <p class="lead"><span class="muted">Scalability and concurrency</span> - Any program can run concurrently with little to no effort by the programmer. Scaling is done through concurrency.</p>
    <p class="lead"><span class="muted">Assured backwards compatibility</span> - Any changes in the framework can be regression tested against all customer created programs.</p>
    <p class="lead"><span class="muted">Immediate feedback</span> - The results of programming are seen immediately within the environment itself: no need to 'code, compile, run and debug'.</p>
    <p class="lead"><span class="muted">Regression testing</span> - Regression testing is an integral part of the development experience.</p>
    <p class="lead"><span class="muted">Separation of mechanisms from business</span> - Assure that programmers can't mix mechanisms and business behavior.</p>
    <p class="lead"><span class="muted">Multiple mental-models</span> - Domain experts have different ways of seeing and understanding what it is they are trying to do. As such, the same program can be represented using different mental models.</p>
  </div>

  <hr class="featurette-divider">

</div>



