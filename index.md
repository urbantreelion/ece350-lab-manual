---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

title: Communications Theory and Systems I
layout: home
---
<div align="right">
<a style="border:1px solid grey; border-radius:5px; padding:5px" href="{{site.github.repository_url}}/edit/master/{{page.path}}">Improve this page</a>
</div>

## Introduction

This lab is meant to be a supplement to your ECE350 class. It will provide you with a way to explore the concepts and techniques discussed in class in a more hands on way. In order to do this, we will be using [GNU Radio Companion](https://wiki.gnuradio.org/index.php/GNURadioCompanion), a graphical tool used to create and execute signal processing flowgraphs for Software Defined Radio. This is a very useful tool not only for demonstrating existing communication methods, but also for experimenting and developing new methods. It is heavily used both in the academic and professional worlds. For more information on Software Defined Radio, have a look at [this Wikipedia page](https://en.wikipedia.org/wiki/Software-defined_radio), or speak to your professor or TA.

This lab manual will regularly refer to the course textbook, available for free [here](./_docs/pdriessen_textbook.pdf).

### Deliverables

The lab component of this course consists of four laboratories:

- Lab 1. GNU Radio tutorials.
- Lab 2. [USRP](http://en.wikipedia.org/wiki/Universal_Software_Radio_Peripheral) tutorials and FM receiver.
- Lab 3. Finding frame synchronization on the [FLEX](<http://en.wikipedia.org/wiki/FLEX_(protocol)>) pager network using the USRP and GNU Radio.
- Lab 4. Pulse shaping and PSK.
- Optional activity:
  - Pass the amateur radio basic and advanced exams (contact Dr. Driessen).
  - Decoding off-air signals not covered in the other lab activities.

The deliverables are described at the beginning of each section.

> NOTE: You are encouraged to work at your own pace, using the scheduled lab period as a time to get assistance from a TA or the course instructor.

[**Begin lab 1**](_lab1/introduction.md)

---

## Revision history

Version | Date | Author | Changelog
------- | ---- | ------ | ---------
**3.1** | ongoing | nsbruce | - Rebuilding lab instructions to smooth the workflow.<br> - Updating all flowgraphs and instructions to reflect GR update from WX to QT.<br> - Restructuring deliverables.
**3.0** | 5 Jun 2019 | nbruce | - Major rebuild. Site has moved from a UVic local SVN repo to Github.<br> - It used to be compiled using Docbook with Apache Ant and Webhelp, it is now running on Github Pages using Jekyll.<br> - All of the lab components have been turned into markdown documents.
**2.1** | 1 Jan 2017 | pkremler | - Updated text references to 2016 edition, clarified lab procedures and reformatted for readability.
**2.0** | 1 Sept 2014 | sharrison |- Updated for 2014 deleting Softrock, retaining 4 labs only.
**1.2** | 4 Jul 2013 | sharrison | - Updated for GNU Radio 3.7.0. The category structure was revised yet again, text updated.
**1.1** | 27 Jun 2013 | sharrison | - Updated for GNU Radio 3.6.5. The category structure was revised quite a bit from earlier versions so the text has been updated to reflect that.
**1.0** | 08 Mar 2013 | sharrison | - Updated for GNU Radio 3.6.3.
