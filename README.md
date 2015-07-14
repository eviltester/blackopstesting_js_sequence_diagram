Black Ops Testing JS-Sequence-Diagram Webinar
======================================

Alan Richardson's exploratory testing notes for [JS-Sequence-Diagram](https://bramp.github.io/js-sequence-diagrams):

* [compendiumdev.co.uk](http://www.compendiumdev.co.uk)
* [eviltester.com](http://www.eviltester.com)
* [seleniumsimplified.com](http://www.seleniumsimplified.com)
* [javafortesters.com](http://www.javafortesters.com)

Exploratory testing notes for the Black Ops Testing Webinar for js-sequence-diagram on 13th July 2015

* [BlackOpsTesting.com](http://blackopstesting.com)

For this Webinar we were testing [JS-Sequence-Diagram](https://bramp.github.io/js-sequence-diagrams)

You will see:

* \notes
  * My notes, which I made in Evernote, in markdown as a pdf so you can see the approach I took for embedding images as metadata in my report which were not used in the final exported report
  * My notes, exported as pdf from [dillinger.io](http://dillinger.io/) so you can see the reason for writing in markdown
  * notes.md - my notes copied to a sublime edited text file from evernote (the unicode chars may not be properly visible in certain editors)
* \tools
  * The tools I wrote to support the testing.
  * crossBrowserTestPage.html - a page with the diagrams representing the issues and examples from the report. I created this simple JavaScript page to run cross browser to allow me to check the diagram rendering on different browsers more easily
  * reporting.html - basically crossBrowserTestPage.html without any real examples in it, for future use
  * explorer.html - a version of test.html from the js-sequence-diagrams project which renders using simple and handrawn format, and keeps a historical record of all the diagrams that you render. Note: saving the page for a session record only works on Firefox and Chrome - IE trims out all the stuff added dynamically to the DOM

This should act as a supplement when you watch the webinar, and hopefully as a standalone example of some exploratory testing and learning documentation approaches.




