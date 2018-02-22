::: {#navbar .navbar .section name="Navbar"}
::: {#Navbar1 .widget .Navbar data-version="1"}
::: {#navbar-iframe-container}
:::
:::
:::

::: {.body-fauxcolumns}
::: {.fauxcolumn-outer .body-fauxcolumn-outer}
::: {.cap-top}
::: {.cap-left}
:::

::: {.cap-right}
:::
:::

::: {.fauxborder-left}
::: {.fauxborder-right}
:::

::: {.fauxcolumn-inner}
:::
:::

::: {.cap-bottom}
::: {.cap-left}
:::

::: {.cap-right}
:::
:::
:::
:::

::: {.content}
::: {.content-fauxcolumns}
::: {.fauxcolumn-outer .content-fauxcolumn-outer}
::: {.cap-top}
::: {.cap-left}
:::

::: {.cap-right}
:::
:::

::: {.fauxborder-left}
::: {.fauxborder-right}
:::

::: {.fauxcolumn-inner}
:::
:::

::: {.cap-bottom}
::: {.cap-left}
:::

::: {.cap-right}
:::
:::
:::
:::

::: {.content-outer}
::: {.content-cap-top .cap-top}
::: {.cap-left}
:::

::: {.cap-right}
:::
:::

::: {.fauxborder-left .content-fauxborder-left}
::: {.fauxborder-right .content-fauxborder-right}
:::

::: {.content-inner}
::: {.header-outer}
::: {.header-cap-top .cap-top}
::: {.cap-left}
:::

::: {.cap-right}
:::
:::

::: {.fauxborder-left .header-fauxborder-left}
::: {.fauxborder-right .header-fauxborder-right}
:::

::: {.region-inner .header-inner}
::: {#header .header .section name="Header"}
::: {#Header1 .widget .Header data-version="1"}
::: {#header-inner}
::: {.titlewrapper}
:::

::: {.descriptionwrapper}
:::
:::
:::
:::
:::
:::

::: {.header-cap-bottom .cap-bottom}
::: {.cap-left}
:::

::: {.cap-right}
:::
:::
:::

::: {.tabs-outer}
::: {.tabs-cap-top .cap-top}
::: {.cap-left}
:::

::: {.cap-right}
:::
:::

::: {.fauxborder-left .tabs-fauxborder-left}
::: {.fauxborder-right .tabs-fauxborder-right}
:::

::: {.region-inner .tabs-inner}
::: {#crosscol .tabs .no-items .section name="Cross-Column"}
:::

::: {#crosscol-overflow .tabs .no-items .section name="Cross-Column 2"}
:::
:::
:::

::: {.tabs-cap-bottom .cap-bottom}
::: {.cap-left}
:::

::: {.cap-right}
:::
:::
:::

::: {.main-outer}
::: {.main-cap-top .cap-top}
::: {.cap-left}
:::

::: {.cap-right}
:::
:::

::: {.fauxborder-left .main-fauxborder-left}
::: {.fauxborder-right .main-fauxborder-right}
:::

::: {.region-inner .main-inner}
::: {.columns .fauxcolumns}
::: {.fauxcolumn-outer .fauxcolumn-center-outer}
::: {.cap-top}
::: {.cap-left}
:::

::: {.cap-right}
:::
:::

::: {.fauxborder-left}
::: {.fauxborder-right}
:::

::: {.fauxcolumn-inner}
:::
:::

::: {.cap-bottom}
::: {.cap-left}
:::

::: {.cap-right}
:::
:::
:::

::: {.fauxcolumn-outer .fauxcolumn-left-outer}
::: {.cap-top}
::: {.cap-left}
:::

::: {.cap-right}
:::
:::

::: {.fauxborder-left}
::: {.fauxborder-right}
:::

::: {.fauxcolumn-inner}
:::
:::

::: {.cap-bottom}
::: {.cap-left}
:::

::: {.cap-right}
:::
:::
:::

::: {.fauxcolumn-outer .fauxcolumn-right-outer}
::: {.cap-top}
::: {.cap-left}
:::

::: {.cap-right}
:::
:::

::: {.fauxborder-left}
::: {.fauxborder-right}
:::

::: {.fauxcolumn-inner}
:::
:::

::: {.cap-bottom}
::: {.cap-left}
:::

::: {.cap-right}
:::
:::
:::

::: {.columns-inner}
::: {.column-center-outer}
::: {.column-center-inner}
::: {#main .main .section name="Main"}
::: {#Blog1 .widget .Blog data-version="1"}
::: {.blog-posts .hfeed}
::: {.date-outer}
## Thursday, July 23, 2015 {#thursday-july-23-2015 .date-header}

::: {.date-posts}
::: {.post-outer}
::: {.post .hentry .uncustomized-post-template itemprop="blogPost" itemscope="itemscope" itemtype="http://schema.org/BlogPosting"}
[]{#1079963996362300797}

### Archimate - Layer 2 Network Diagrams in Archi {#archimate---layer-2-network-diagrams-in-archi .post-title .entry-title itemprop="name"}

::: {.post-header}
::: {.post-header-line-1}
:::
:::

::: {#post-body-1079963996362300797 .post-body .entry-content itemprop="description articleBody"}
> Note: I am going to make many references to Archimate and Archi in
> this article. You can read up on the Archimate standard at this link:
> <http://pubs.opengroup.org/architecture/archimate2-doc/> and you can
> read up on Archi (a tool for creating Archimate models and views) at
> this link: <http://www.archimatetool.com/>. If you are unfamiliar with
> network layers you may also need to perform some research on that
> topic. This turned out to be a longer article than I thought, so I am
> breaking it into two parts. Layer 2 will be done in this article,
> Layer 3 in the next. I'll update this article with a link after I
> publish that one.

I have been using Archimate for some time now. I primarily use Archi for
creating models and views as needed on a day to day basis. I got
certified last year after using the standard and various tools for about
a year or so and have been using it regularly ever since.

One of the items that has plagued me for some time is how to represent
network architecture. Frequently I am working with more conceptual
stuff, but when I do want to show networking, what should I do?
Interfaces or not? Networks or Communication Paths? Nodes vs Devices? I
have made various choices based on need in the past. I thought I would
take some time, work through one with more intention, and try to make a
few decisions.

An article on packetpushers served as a source of inspiration and also
as a source for the sample network diagram. This is a great article that
shows how to take config information from network devices and turn it
into a layer 3 diagram. It also includes a great example of a Layer 2
diagram.

Link:
<http://packetpushers.net/how-to-draw-clear-l3-logical-network-diagrams/>

These appear to be Visio diagrams, so how should I turn these into
Archimate Models? My preference is to be model driven which means
elements and relationships come first. This means lists of things. That
is what the 'model' is basically. Lists of elements and Lists of
relationships. There is a very real attraction to dive into the view
creation and perform data entry that way, but I think more gets done if
you start with the model and move forward. At least I generally feel
like I uncover gaps in my assumptions and end up with a more solid
model. Normally after creating the elements, I can dive into the
relationships if needed to get some kind of work in progress to
evaluate.

So let's start with Layer 2 since that's all device and ports and such.
Should be easier, no?

I'll use this diagram:

![](http://packetpushers.net/wp-content/uploads/2012/12/Example-network2.jpg){width="672"
height="768"}

Source:
<http://packetpushers.net/wp-content/uploads/2012/12/Example-network2.jpg>

First let's see what kind of elements we are going to use. Looking at
this picture we have the following unique elements:

1.  Some Devices
2.  Lines connecting them
3.  Some labels on the lines representing different things
4.  Po1 Labels which seem to be different than the other labels

We'll map these to the Archimate elements like so:

1.  Some Devices -- We'll just make these 'Device' Elements
2.  Lines connecting them -- We'll just make these 'Association'
    Relationships
3.  Some labels on the lines representing different things -- We'll make
    these 'Interface' Elements on the Devices
4.  Po1 Labels which seem to be different than the other labels -- I'm
    not sure what these are, so we'll ignore them for now

Why these items? You can look at the definitions for the items on the
Archimate standard site.

Let's make a list of the Archimate Elements. First Devices, and then
Interfaces. The interfaces all "belong" to certain devices and do not
have unique names, so you will have to list them with a parent.

Devices:

  -----------
  Inet-rtr1
  Inet-rtr2
  Outsw1
  Outsw2
  Fw1
  Fw2
  Csw1
  Csw2
  Asw1
  Asw2
  Asw3
  -----------

Interfaces:

  ----------- -----------
  Parent      Interface
  Inet-rtr1   G0/0
  Inet-rtr2   G0/0
  Outsw1      G1/0
  Outsw1      G1/1
  Outsw1      G1/3
  Outsw1      G1/4
  Outsw2      G1/0
  Outsw2      G1/1
  Outsw2      G1/3
  Outsw2      G1/4
  Fw1         E0/1
  Fw1         E0/2
  Fw1         E0/3
  Fw2         E0/1
  Fw2         E0/2
  Fw2         E0/3
  Csw1        G0/1
  Csw1        G0/2
  Csw1        G0/3
  Csw1        G0/4
  Csw1        G0/5
  Csw1        G0/6
  Csw2        G0/1
  Csw2        G0/2
  Csw2        G0/3
  Csw2        G0/4
  Csw2        G0/5
  Csw2        G0/6
  Asw1        G0/1
  Asw1        G0/2
  Asw2        G0/1
  Asw2        G0/2
  Asw3        G0/1
  Asw3        G0/2
  ----------- -----------

Now the association relationships for the lines:

  ----------- ----------- --------------- ----------- -----------
  Parent      Interface   \< \-- \-- \>   Parent      Interface
  Inet-rtr1   G0/0        x               Outsw1      G1/0
  Inet-rtr2   G0/0        x               Outsw2      G1/0
  Outsw1      G1/0        x               Inet-rtr1   G0/0
  Outsw1      G1/1        x               Fw1         E0/1
  Outsw1      G1/3        x               Outsw2      G1/3
  Outsw1      G1/4        x               Outsw2      G1/4
  Outsw2      G1/0        x               Inet-rtr2   G0/0
  Outsw2      G1/1        x               Fw2         E0/1
  Outsw2      G1/3        x               Outsw1      G1/3
  Outsw2      G1/4        x               Outsw1      G1/4
  Fw1         E0/1        x               Outsw1      G1/1
  Fw1         E0/2        x               Csw1        G0/1
  Fw1         E0/3        x               Fw2         E0/3
  Fw2         E0/1        x               Outsw2      G1/1
  Fw2         E0/2        x               Csw2        G0/1
  Fw2         E0/3        x               Fw1         E0/3
  Csw1        G0/1        x               Fw1         E0/2
  Csw1        G0/2        x               Csw2        G0/2
  Csw1        G0/3        x               Csw2        G0/3
  Csw1        G0/4        x               Asw1        G0/1
  Csw1        G0/5        x               Asw2        G0/1
  Csw1        G0/6        x               Asw3        G0/1
  Csw2        G0/1        x               Fw2         E0/2
  Csw2        G0/2        x               Csw1        G0/2
  Csw2        G0/3        x               Csw1        G0/3
  Csw2        G0/4        x               Asw1        G0/2
  Csw2        G0/5        x               Asw2        G0/2
  Csw2        G0/6        x               Asw3        G0/2
  Asw1        G0/1        x               Csw1        G0/4
  Asw1        G0/2        x               Csw2        G0/4
  Asw2        G0/1        x               Csw1        G0/5
  Asw2        G0/2        x               Csw2        G0/5
  Asw3        G0/1        x               Csw1        G0/6
  Asw3        G0/2        x               Csw2        G0/6
  ----------- ----------- --------------- ----------- -----------

Now, there are some 'duplicate' relationships here. We'll deal with this
in a moment. For now we have all of our initial elements and
relationships. Let's go ahead and get these into Archi.

To do this I am going to use the following 'empty' model.

    <?xml version="1.0" encoding="UTF-8"?>

    <archimate:model xmlns:archimate="http://www.archimatetool.com/archimate" name="EmptyModel" version="3.1.0">

      <folder name="Business" type="business"/>

      <folder name="Application" type="application"/>

      <folder name="Technology" type="technology"/>

      <folder name="Motivation" type="motivation"/>

      <folder name="Implementation &amp; Migration" type="implementation_migration"/>

      <folder name="Connectors" type="connectors"/>

      <folder name="Relations" type="relations"/>

      <folder name="Views" type="diagrams"/>

    </archimate:model>

\

This is basically just the folder structure of the default archi model.
I am going to add some elements to it. Archi will add IDs to everything
once we open it and save it, so for now each item will be unique just
because I'm creating multiple copies. You can add your own IDs if you
want and that may be preferable in some instances. If you open a new
empty model in Archi and save it with no changes, it will look like this
but it will have IDs in it. I just happen to have an empty model that I
once upon a time saved so I could insert data. I should also note, now,
that the newest version of Archi supports importing CSV files. Those
files require a special format and you can look at Archi site if you
want to take that route.\

First let's add all of our Devices. To keep things a bit easier to
manage, I will put these in their own folder element. These are all
Technology items so I am going to put them in the default folder for
that.

    <?xml version="1.0" encoding="UTF-8"?>

    <archimate:model xmlns:archimate="http://www.archimatetool.com/archimate" name="EmptyModel" version="3.1.0">

      <folder name="Business" type="business"/>

      <folder name="Application" type="application"/>

      <folder name="Technology" type="technology">

           <folder name="Devices">

                  <element xsi:type="archimate:Device" name="Inet-rtr1" />

                  <element xsi:type="archimate:Device" name="Inet-rtr2" />

                  <element xsi:type="archimate:Device" name="Outsw1" />

                  <element xsi:type="archimate:Device" name="Outsw2" />

                  <element xsi:type="archimate:Device" name="Fw1" />

                  <element xsi:type="archimate:Device" name="Fw2" />

                  <element xsi:type="archimate:Device" name="Csw1" />

                  <element xsi:type="archimate:Device" name="Csw2" />

                  <element xsi:type="archimate:Device" name="Asw1" />

                  <element xsi:type="archimate:Device" name="Asw2" />

                  <element xsi:type="archimate:Device" name="Asw3" />

           </folder>

      </folder>

      <folder name="Motivation" type="motivation"/>

      <folder name="Implementation &amp; Migration" type="implementation_migration"/>

      <folder name="Connectors" type="connectors"/>

      <folder name="Relations" type="relations"/>

      <folder name="Views" type="diagrams"/>

    </archimate:model>

\

Next we should plug in our interfaces. I am going to adopt the naming
convention of \[device\]interfacename for now so I can keep track of
what is related to what.

    <?xml version="1.0" encoding="UTF-8"?>

    <archimate:model xmlns:archimate="http://www.archimatetool.com/archimate" name="EmptyModel" version="3.1.0">

      <folder name="Business" type="business"/>

      <folder name="Application" type="application"/>

      <folder name="Technology" type="technology">

           <folder name="Devices">

                  <element xsi:type="archimate:Device" name="Inet-rtr1" />

                  <element xsi:type="archimate:Device" name="Inet-rtr2" />

                  <element xsi:type="archimate:Device" name="Outsw1" />

                  <element xsi:type="archimate:Device" name="Outsw2" />

                  <element xsi:type="archimate:Device" name="Fw1" />

                  <element xsi:type="archimate:Device" name="Fw2" />

                  <element xsi:type="archimate:Device" name="Csw1" />

                  <element xsi:type="archimate:Device" name="Csw2" />

                  <element xsi:type="archimate:Device" name="Asw1" />

                  <element xsi:type="archimate:Device" name="Asw2" />

                  <element xsi:type="archimate:Device" name="Asw3" />

           </folder>

           <folder name="Interfaces">

                  <element xsi:type="archimate:InfrastructureInterface" name="[Inet-rtr1]G0/0" />

                  <element xsi:type="archimate:InfrastructureInterface" name="[Inet-rtr2]G0/0" />

                  <element xsi:type="archimate:InfrastructureInterface" name="[Outsw1]G1/0" />

                  <element xsi:type="archimate:InfrastructureInterface" name="[Outsw1]G1/1" />

                  <element xsi:type="archimate:InfrastructureInterface" name="[Outsw1]G1/3" />

                  <element xsi:type="archimate:InfrastructureInterface" name="[Outsw1]G1/4" />

                  <element xsi:type="archimate:InfrastructureInterface" name="[Outsw2]G1/0" />

                  <element xsi:type="archimate:InfrastructureInterface" name="[Outsw2]G1/1" />

                  <element xsi:type="archimate:InfrastructureInterface" name="[Outsw2]G1/3" />

                  <element xsi:type="archimate:InfrastructureInterface" name="[Outsw2]G1/4" />

                  <element xsi:type="archimate:InfrastructureInterface" name="[Fw1]E0/1" />

                  <element xsi:type="archimate:InfrastructureInterface" name="[Fw1]E0/2" />

                  <element xsi:type="archimate:InfrastructureInterface" name="[Fw1]E0/3" />

                  <element xsi:type="archimate:InfrastructureInterface" name="[Fw2]E0/1" />

                  <element xsi:type="archimate:InfrastructureInterface" name="[Fw2]E0/2" />

                  <element xsi:type="archimate:InfrastructureInterface" name="[Fw2]E0/3" />

                  <element xsi:type="archimate:InfrastructureInterface" name="[Csw1]G0/1" />

                  <element xsi:type="archimate:InfrastructureInterface" name="[Csw1]G0/2" />

                  <element xsi:type="archimate:InfrastructureInterface" name="[Csw1]G0/3" />

                  <element xsi:type="archimate:InfrastructureInterface" name="[Csw1]G0/4" />

                  <element xsi:type="archimate:InfrastructureInterface" name="[Csw1]G0/5" />

                  <element xsi:type="archimate:InfrastructureInterface" name="[Csw1]G0/6" />

                  <element xsi:type="archimate:InfrastructureInterface" name="[Csw2]G0/1" />

                  <element xsi:type="archimate:InfrastructureInterface" name="[Csw2]G0/2" />

                  <element xsi:type="archimate:InfrastructureInterface" name="[Csw2]G0/3" />

                  <element xsi:type="archimate:InfrastructureInterface" name="[Csw2]G0/4" />

                  <element xsi:type="archimate:InfrastructureInterface" name="[Csw2]G0/5" />

                  <element xsi:type="archimate:InfrastructureInterface" name="[Csw2]G0/6" />

                  <element xsi:type="archimate:InfrastructureInterface" name="[Asw1]G0/1" />

                  <element xsi:type="archimate:InfrastructureInterface" name="[Asw1]G0/2" />

                  <element xsi:type="archimate:InfrastructureInterface" name="[Asw2]G0/1" />

                  <element xsi:type="archimate:InfrastructureInterface" name="[Asw2]G0/2" />

                  <element xsi:type="archimate:InfrastructureInterface" name="[Asw3]G0/1" />

                  <element xsi:type="archimate:InfrastructureInterface" name="[Asw3]G0/2" />

           </folder>

     </folder>

      <folder name="Motivation" type="motivation"/>

      <folder name="Implementation &amp; Migration" type="implementation_migration"/>

      <folder name="Connectors" type="connectors"/>

      <folder name="Relations" type="relations"/>

      <folder name="Views" type="diagrams"/>

    </archimate:model>

\

This is where things get a little fun. You have a decision. Do you want
to create relationships in this manner via hardcoding, or do you want to
go into the GUI and drag and drop things. Drag and drop takes a little
bit, but it may be faster overall. Creating relationships in Archi
requires an ID for each element in the XML.  You can create IDs that
match names, etc to make it easier but it is still a little effort. You
can also just open the .archimate file in the tool and save it. It will
put generated IDs in there. Then you will have to create relationship
elements for each item you want to relate.\

I am just going to use the GUI. To create this I simply opened Archi and
dragged all of the elements in the model onto a single canvas. Then I
ordered and resized some of the elements as below. Then I drew
composition relationships between the devices and their interfaces and I
drew association relationships between the interfaces. It's worth
mentioning that association relationships in Archi are directional.
Meaning a relationship between A and B is not the same as one between B
and A. There is always a source and target. I am not going to worry
about that for this exercise so I just structured my view after the
source picture and drew relationships from top to bottom and left to
right. If I were to do this via hardcoding, I would probably add both
association relationships. Here is the first draft.\

[![clip\_image002](http://lh3.googleusercontent.com/-KuBoCwIyRv4/VbFfegwE-VI/AAAAAAAACAk/_9gsfUligGQ/clip_image002_thumb%25255B5%25255D.jpg?imgmax=800 "clip_image002"){width="731"
height="768"}](http://lh3.googleusercontent.com/-SxVbjWgop9U/VbFfeNKzI-I/AAAAAAAACAY/K0W8BdJ6hVs/s1600-h/clip_image002%25255B11%25255D.jpg)\

This works fine. You can actually stop here if you like. The data is in
the model and you could layer things on top of this if needed. But I'm
not sure I like the 'messiness' of the parent component being in the
name of each interface. What element it belongs to is really handled by
the 'compose' relationship anyway. Redundancy! So I am going to change
the names to make things a bit cleaner like so:\

[![clip\_image003](http://lh3.googleusercontent.com/-yer9SR13sck/VbFffRFh24I/AAAAAAAACA8/qgafetOA3yA/clip_image003_thumb%25255B4%25255D.jpg?imgmax=800 "clip_image003"){width="731"
height="768"}](http://lh3.googleusercontent.com/-dNOjbXpsQbA/VbFffPjKk3I/AAAAAAAACAw/PZJPBwoHang/s1600-h/clip_image003%25255B7%25255D.jpg)\

After this, I wanted to get a little closer to existing document. I
ended up with the view below.\

[![clip\_image004](http://lh3.googleusercontent.com/-7qMS8NIji1g/VbFfgN7tZyI/AAAAAAAACBU/9UMSoaf_1qw/clip_image004_thumb%25255B4%25255D.jpg?imgmax=800 "clip_image004"){width="362"
height="480"}](http://lh3.googleusercontent.com/-JbH8AX9MVTg/VbFff0bIf2I/AAAAAAAACBI/ISaufl45hAQ/s1600-h/clip_image004%25255B9%25255D.jpg)\

One of the gaps in translating a document like this into Archimate is
that you can't tell what is a router, firewall, etc in the view. You can
enter custom user data, which is supported by the standard, if you like.
Say I create a field called 'Type' and on a Device element I can put
Router, Switch, Firewall, PC, Server, etc. I am going to use a different
method that I use for servers. I will just create a Device called
whatever I want, and I will make these items specializations of that
item. Like so:\

[![clip\_image005](http://lh3.googleusercontent.com/-yi8znl4TcSE/VbFfg37d1ZI/AAAAAAAACBo/5DYbLH4s_r0/clip_image005_thumb%25255B3%25255D.jpg?imgmax=800 "clip_image005"){width="573"
height="480"}](http://lh3.googleusercontent.com/-pfoU51vv60M/VbFfgeYhsgI/AAAAAAAACBg/_JO07TGvTIM/s1600-h/clip_image005%25255B6%25255D.jpg)\

Now this view is very ugly. But the relationships are sound. I can put
whatever notes I want on a Router Device without having to put them on
each one. I don't want to show the Device that is specialized in this
view, but I want to provide a visual cue, so I am going to simply use
color coding and a note. The model doesn't need visual elements because
the data is in the relationships and the elements themselves. I know
that ASW3 is a switch because it is a specialization of a device called
a switch. But for the view, I need to see some info. Like so:\

[![clip\_image002\[7\]](http://lh3.googleusercontent.com/-bVa0ooR7V4g/VbFfhl3jK-I/AAAAAAAACCE/58bwhY9pB5o/clip_image002%25255B7%25255D_thumb%25255B8%25255D.jpg?imgmax=800 "clip_image002[7]"){width="495"
height="768"}](http://lh3.googleusercontent.com/-GV_ELuknMgo/VbFfhQYXgbI/AAAAAAAACB0/S_O8lq1Bifk/s1600-h/clip_image002%25255B7%25255D%25255B11%25255D.jpg)\

And this is our final Layer 2 Diagram. I am not sure what the Po1 sets
were, but if they were paired channels, I would simply have the pairing
interfaces compose a paired interface and link those. If you are trying
this at home and wondering how I got everything to be the same color
without clicking on each device, I used the Format Painter on the
Palette in Archi. Another quick tip if you need to perform a ton of
connections of the same type is to hold SHIFT before you click on the
relationship in the Palette. Then you can use it over and over again
until you hit ESC or click out of it in some way.\

Actually, as a final touch, I normally add some notes and a title to my
view. At least the ones that I actually mean people to 'look at' rather
than one that is a working view for me to model in. Here is my final
update.\

[![clip\_image004\[5\]](http://lh3.googleusercontent.com/-bdjnkRet9ho/VbFfiXzrt4I/AAAAAAAACCc/QPwbfgUDMVs/clip_image004%25255B5%25255D_thumb%25255B4%25255D.jpg?imgmax=800 "clip_image004[5]"){width="796"
height="768"}](http://lh3.googleusercontent.com/-A2Q89Qn0iyA/VbFfh4Baq7I/AAAAAAAACCQ/7tBAvNMl1qg/s1600-h/clip_image004%25255B5%25255D%25255B6%25255D.jpg)\

The notes are just whatever I thought would be relevant at the time. If
I were going to 'present this' it is likely I would always put the data
back into Visio, PowerPoint, or some other 'prettier' tool. But if you
need to get the L2 data into your model, this is how I would do it.\

This blog post has become longer than I thought it would be, so I am
making it Layer 2 only and will do the Layer 3 mapping in another one.\

[![image](http://lh3.googleusercontent.com/-3kpX9wfAews/VbFlnvHAgjI/AAAAAAAACCw/B_0OxxkF-EA/image_thumb%25255B6%25255D.png?imgmax=800 "image"){width="1024"
height="705"}](http://lh3.googleusercontent.com/-5jJ3IhWXffU/VbFlnE4DEDI/AAAAAAAACCk/hef7yMZnvS0/s1600-h/image%25255B10%25255D.png)

::: {style="clear: both;"}
:::
:::

::: {.post-footer}
::: {.post-footer-line .post-footer-line-1}
[ ]{.post-author .vcard} [ at ]{.post-timestamp}

[July 23,
2015](http://blog.royashbrook.com/2015/07/archimate-layer-2-network-diagrams-in.html "permanent link"){.timestamp-link}
[ ]{.reaction-buttons} [ ]{.post-comment-link} [ ]{.post-backlinks
.post-comment-link} [ [
[![](https://resources.blogblog.com/img/icon18_edit_allbkg.gif){.icon-action
width="18"
height="18"}](https://www.blogger.com/post-edit.g?blogID=1453735247586098860&postID=1079963996362300797&from=pencil "Edit Post")
]{.item-control .blog-admin .pid-861436283} ]{.post-icons}

::: {.post-share-buttons .goog-inline-block}
[[Email
This]{.share-button-link-text}](https://www.blogger.com/share-post.g?blogID=1453735247586098860&postID=1079963996362300797&target=email "Email This"){.goog-inline-block
.share-button
.sb-email}[[BlogThis!]{.share-button-link-text}](https://www.blogger.com/share-post.g?blogID=1453735247586098860&postID=1079963996362300797&target=blog "BlogThis!"){.goog-inline-block
.share-button .sb-blog}[[Share to
Twitter]{.share-button-link-text}](https://www.blogger.com/share-post.g?blogID=1453735247586098860&postID=1079963996362300797&target=twitter "Share to Twitter"){.goog-inline-block
.share-button .sb-twitter}[[Share to
Facebook]{.share-button-link-text}](https://www.blogger.com/share-post.g?blogID=1453735247586098860&postID=1079963996362300797&target=facebook "Share to Facebook"){.goog-inline-block
.share-button .sb-facebook}[[Share to
Pinterest]{.share-button-link-text}](https://www.blogger.com/share-post.g?blogID=1453735247586098860&postID=1079963996362300797&target=pinterest "Share to Pinterest"){.goog-inline-block
.share-button .sb-pinterest}

::: {.goog-inline-block .google-plus-share-container}
:::
:::
:::

::: {.post-footer-line .post-footer-line-2}
[ Labels: [Archi](http://blog.royashbrook.com/search/label/Archi),
[Archimate](http://blog.royashbrook.com/search/label/Archimate),
[Networking](http://blog.royashbrook.com/search/label/Networking)
]{.post-labels}
:::

::: {.post-footer-line .post-footer-line-3}
[ ]{.post-location}
:::
:::
:::

::: {#comments .comments}
[]{#comments}

#### 2 comments:

::: {.comments-content}
::: {#comment-holder}
::: {#bc_0_3C kind="c"}
::: {#bc_0_3CT}
::: {#bc_0_2T .comment-thread kind="r" t="0" u="0"}
1.  ::: {#bc_0_0B}
    ::: {.avatar-image-container}
    ![](//lh5.googleusercontent.com/-aGYa49bHJC4/AAAAAAAAAAI/AAAAAAAABAw/BiqmlQd3mSA/s35-c/photo.jpg)
    :::

    ::: {#c6850828437514721159 .comment-block}
    ::: {#bc_0_0M .comment-header kind="m"}
    [Jason
    Burman](https://www.blogger.com/profile/02252706209753568176)[]{.icon
    .user}[[October 16, 2016 at 9:17
    PM](http://blog.royashbrook.com/2015/07/archimate-layer-2-network-diagrams-in.html?showComment=1476677825442#c6850828437514721159)]{.datetime
    .secondary-text}
    :::

    Hi,\
    \
    As you mentioned, the model that you present does not address the
    Po1 labels in the diagram. This is a Cisco Port Channel grouping.\
    \
    A Port Channel group aggregates two or more physical interfaces into
    a single logical interface. The switches at each side of the channel
    then see the G0/2 & G0/3 interfaces as one Po1 Ethernet port.\
    \
    This has the characteristics of:\
    \* Aggregated bandwidth (i.e. two 1Gbps physical interfaces will
    combine to create a logical 2Gbps interface)\
    \* Some redundancy is introduced in that if one of the physical
    interfaces fails, the logical interface remains active (albeit only
    operating at the line speed of 1Gbps).\
    \
    So, with this information:\
    \* How would the redundancy be shown in Archimate?\
    \* How would the link aggregation be shown in Archimate?\
    \
    TIA\
    Jason

    [[Reply](javascript:;)[[Delete](https://www.blogger.com/delete-comment.g?blogID=1453735247586098860&postID=6850828437514721159)]{.item-control
    .blog-admin .pid-102024248}]{#bc_0_0MN .comment-actions
    .secondary-text kind="m"}
    :::

    ::: {#bc_0_0BR .comment-replies}
    :::

    ::: {#bc_0_0B_box .comment-replybox-single}
    :::
    :::

2.  ::: {#bc_0_1B}
    ::: {.avatar-image-container}
    ![](//3.bp.blogspot.com/-grjeuuBrWrQ/VbFvQRVSIjI/AAAAAAAACD8/vvTHTjtcTak/s35/*)
    :::

    ::: {#c3702503518087364787 .comment-block}
    ::: {#bc_0_1M .comment-header kind="m"}
    [royashbrook](https://www.blogger.com/profile/15954039149100890352)[]{.icon
    .user .blog-author}[[May 15, 2017 at 2:36
    PM](http://blog.royashbrook.com/2015/07/archimate-layer-2-network-diagrams-in.html?showComment=1494884203708#c3702503518087364787)]{.datetime
    .secondary-text}
    :::

    Jason, I never replied to your comment as I wasn\'t 100% sure at the
    time. The goal of this exercise was to make some decisions on how to
    represent some of these items and after completion, I wasn\'t
    convinced on how I would address the channel groupings after
    researching it more later. I think I would still leave them off of a
    \'simpler\' view, but given this was meant to be a 1-1 mapping from
    one diagram into a model I would add them if I went back to re-model
    this. I would just have the port channel interfaces composed of the
    physical interfaces then add the details on that new interface for
    redundancy, etc.

    [[Reply](javascript:;)[[Delete](https://www.blogger.com/delete-comment.g?blogID=1453735247586098860&postID=3702503518087364787)]{.item-control
    .blog-admin .pid-861436283}]{#bc_0_1MN .comment-actions
    .secondary-text kind="m"}
    :::

    ::: {#bc_0_1BR .comment-replies}
    :::

    ::: {#bc_0_1B_box .comment-replybox-single}
    :::
    :::

::: {#bc_0_2I .continue kind="ci"}
[Add comment](javascript:;)
:::

::: {#bc_0_2T_box .comment-replybox-thread}
:::

::: {#bc_0_2L .loadmore .loaded kind="rb"}
[Load more\...](javascript:;)
:::
:::
:::
:::
:::
:::

::: {.comment-form}
[]{#comment-form}

[](https://www.blogger.com/comment-iframe.g?blogID=1453735247586098860&postID=1079963996362300797){#comment-editor-src}
:::

::: {#backlinks-container}
::: {#Blog1_backlinks-container}
:::
:::
:::
:::
:::
:::
:::

::: {#blog-pager .blog-pager}
[ [Newer
Post](http://blog.royashbrook.com/2017/06/set-wireless-network-connection-order.html "Newer Post"){#Blog1_blog-pager-newer-link
.blog-pager-newer-link} ]{#blog-pager-newer-link}
[Home](http://blog.royashbrook.com/){.home-link}
:::

::: {.clear}
:::

::: {.post-feeds}
::: {.feed-links}
Subscribe to: [Post Comments
(Atom)](http://blog.royashbrook.com/feeds/1079963996362300797/comments/default){.feed-link}
:::
:::
:::

::: {#FeaturedPost1 .widget .FeaturedPost data-version="1"}
::: {.post-summary}
### [Decrypting VNC Passwords](http://blog.royashbrook.com/2018/01/decrypting-vnc-passwords.html)

Situation: I have a very old \'server\' that is running vnc. No one
knows the password, but we have local admin on the machine. The \...
:::

::: {.clear}
:::

[ [
[![](https://resources.blogblog.com/img/icon18_wrench_allbkg.png){width="18"
height="18"}](//www.blogger.com/rearrange?blogID=1453735247586098860&widgetType=FeaturedPost&widgetId=FeaturedPost1&action=editWidget&sectionId=main "Edit"){.quickedit}
]{.item-control .blog-admin} ]{.widget-item-control}

::: {.clear}
:::
:::

::: {#PopularPosts1 .widget .PopularPosts data-version="1"}
::: {.widget-content .popular-posts}
-   ::: {.item-content}
    ::: {.item-thumbnail}
    [![](https://lh5.googleusercontent.com/proxy/O9DX9JhBfwlfKS7nGwvga2NC6rtfH20DWRo8w-bI2z4WQ3YHmjbJXU0vOmwGwW0JlT5nX85o12vHSjvOArafSyM_fxi3sDUVBnhFBtdFnffeMdlVxbbblXMJXPk=w72-h72-p-k-no-nu)](http://blog.royashbrook.com/2015/07/archimate-layer-2-network-diagrams-in.html)
    :::

    ::: {.item-title}
    [Archimate - Layer 2 Network Diagrams in
    Archi](http://blog.royashbrook.com/2015/07/archimate-layer-2-network-diagrams-in.html)
    :::

    ::: {.item-snippet}
    Note: I am going to make many references to Archimate and Archi in
    this article. You can read up on the Archimate standard at this
    link: ht\...
    :::
    :::

    ::: {style="clear: both;"}
    :::

-   ::: {.item-content}
    ::: {.item-title}
    [Encrypting and Decrypting SecureString values in
    Powershell](http://blog.royashbrook.com/2017/08/encrypting-and-decrypting-securestring.html)
    :::

    ::: {.item-snippet}
    :::
    :::

    ::: {style="clear: both;"}
    :::

-   ::: {.item-content}
    ::: {.item-title}
    [Inventory Scheduled Tasks from Windows
    Server](http://blog.royashbrook.com/2017/07/inventory-scheduled-tasks-from-windows.html)
    :::

    ::: {.item-snippet}
    schtasks /query /s SERVERNAME /v /fo list \> jobs.txt Generally use
    /fo csv for analysis, but for a quick peak list seems much easier
    to\...
    :::
    :::

    ::: {style="clear: both;"}
    :::

::: {.clear}
:::

[ [
[![](https://resources.blogblog.com/img/icon18_wrench_allbkg.png){width="18"
height="18"}](//www.blogger.com/rearrange?blogID=1453735247586098860&widgetType=PopularPosts&widgetId=PopularPosts1&action=editWidget&sectionId=main "Edit"){.quickedit}
]{.item-control .blog-admin} ]{.widget-item-control}

::: {.clear}
:::
:::
:::
:::
:::
:::

::: {.column-left-outer}
::: {.column-left-inner}
:::
:::

::: {.column-right-outer}
::: {.column-right-inner}
::: {#sidebar-right-1 .sidebar .section}
::: {#BlogArchive1 .widget .BlogArchive data-version="1"}
## Blog Archive

::: {.widget-content}
::: {#ArchiveList}
::: {#BlogArchive1_ArchiveList}
-   [[ ►  ]{.zippy}](javascript:void(0)){.toggle}
    [2018](http://blog.royashbrook.com/2018/){.post-count-link}
    [(3)]{.post-count dir="ltr"}
    -   [[ ►  ]{.zippy}](javascript:void(0)){.toggle}
        [January](http://blog.royashbrook.com/2018/01/){.post-count-link}
        [(3)]{.post-count dir="ltr"}

<!-- -->

-   [[ ►  ]{.zippy}](javascript:void(0)){.toggle}
    [2017](http://blog.royashbrook.com/2017/){.post-count-link}
    [(10)]{.post-count dir="ltr"}
    -   [[ ►  ]{.zippy}](javascript:void(0)){.toggle}
        [August](http://blog.royashbrook.com/2017/08/){.post-count-link}
        [(4)]{.post-count dir="ltr"}

    <!-- -->

    -   [[ ►  ]{.zippy}](javascript:void(0)){.toggle}
        [July](http://blog.royashbrook.com/2017/07/){.post-count-link}
        [(5)]{.post-count dir="ltr"}

    <!-- -->

    -   [[ ►  ]{.zippy}](javascript:void(0)){.toggle}
        [June](http://blog.royashbrook.com/2017/06/){.post-count-link}
        [(1)]{.post-count dir="ltr"}

<!-- -->

-   [[ ▼  ]{.zippy .toggle-open}](javascript:void(0)){.toggle}
    [2015](http://blog.royashbrook.com/2015/){.post-count-link}
    [(1)]{.post-count dir="ltr"}
    -   [[ ▼  ]{.zippy .toggle-open}](javascript:void(0)){.toggle}
        [July](http://blog.royashbrook.com/2015/07/){.post-count-link}
        [(1)]{.post-count dir="ltr"}
        -   [Archimate - Layer 2 Network Diagrams in
            Archi](http://blog.royashbrook.com/2015/07/archimate-layer-2-network-diagrams-in.html)
:::
:::

::: {.clear}
:::

[ [
[![](https://resources.blogblog.com/img/icon18_wrench_allbkg.png){width="18"
height="18"}](//www.blogger.com/rearrange?blogID=1453735247586098860&widgetType=BlogArchive&widgetId=BlogArchive1&action=editWidget&sectionId=sidebar-right-1 "Edit"){.quickedit}
]{.item-control .blog-admin} ]{.widget-item-control}

::: {.clear}
:::
:::
:::

::: {#Label1 .widget .Label data-version="1"}
## Labels

::: {.widget-content .list-label-widget-content}
-   [Archi](http://blog.royashbrook.com/search/label/Archi)
-   [Archimate](http://blog.royashbrook.com/search/label/Archimate)
-   [Networking](http://blog.royashbrook.com/search/label/Networking)

::: {.clear}
:::

[ [
[![](https://resources.blogblog.com/img/icon18_wrench_allbkg.png){width="18"
height="18"}](//www.blogger.com/rearrange?blogID=1453735247586098860&widgetType=Label&widgetId=Label1&action=editWidget&sectionId=sidebar-right-1 "Edit"){.quickedit}
]{.item-control .blog-admin} ]{.widget-item-control}

::: {.clear}
:::
:::
:::
:::

+-----------------------------------+-----------------------------------+
| ::: {#sidebar-right-2-1 .sidebar  | ::: {#sidebar-right-2-2 .sidebar  |
| .section}                         | .section}                         |
| ::: {#PageList1 .widget .PageList | ::: {#BlogSearch1 .widget .BlogSe |
|  data-version="1"}                | arch data-version="1"}            |
| ::: {.widget-content}             | ## Search This Blog {#search-this |
| -   [Home](http://blog.royashbroo | -blog .title}                     |
| k.com/)                           |                                   |
|                                   | ::: {.widget-content}             |
| ::: {.clear}                      | ::: {#BlogSearch1_form}           |
| :::                               |   -- --                           |
|                                   |                                   |
| [ [                               |   -- --                           |
| [![](https://resources.blogblog.c | :::                               |
| om/img/icon18_wrench_allbkg.png){ | :::                               |
| width="18"                        |                                   |
| height="18"}](//www.blogger.com/r | ::: {.clear}                      |
| earrange?blogID=14537352475860988 | :::                               |
| 60&widgetType=PageList&widgetId=P |                                   |
| ageList1&action=editWidget&sectio | [ [                               |
| nId=sidebar-right-2-1 "Edit"){.qu | [![](https://resources.blogblog.c |
| ickedit}                          | om/img/icon18_wrench_allbkg.png){ |
| ]{.item-control .blog-admin}      | width="18"                        |
| ]{.widget-item-control}           | height="18"}](//www.blogger.com/r |
|                                   | earrange?blogID=14537352475860988 |
| ::: {.clear}                      | 60&widgetType=BlogSearch&widgetId |
| :::                               | =BlogSearch1&action=editWidget&se |
| :::                               | ctionId=sidebar-right-2-2 "Edit") |
| :::                               | {.quickedit}                      |
| :::                               | ]{.item-control .blog-admin}      |
|                                   | ]{.widget-item-control}           |
|                                   |                                   |
|                                   | ::: {.clear}                      |
|                                   | :::                               |
|                                   | :::                               |
|                                   | :::                               |
+-----------------------------------+-----------------------------------+

::: {#sidebar-right-3 .sidebar .no-items .section}
:::
:::
:::
:::

::: {style="clear: both"}
:::
:::
:::
:::

::: {.main-cap-bottom .cap-bottom}
::: {.cap-left}
:::

::: {.cap-right}
:::
:::
:::

::: {.footer-outer}
::: {.footer-cap-top .cap-top}
::: {.cap-left}
:::

::: {.cap-right}
:::
:::

::: {.fauxborder-left .footer-fauxborder-left}
::: {.fauxborder-right .footer-fauxborder-right}
:::

::: {.region-inner .footer-inner}
::: {#footer-1 .foot .no-items .section}
:::

+-----------------------------------+-----------------------------------+
| ::: {#footer-2-1 .foot .no-items  | ::: {#footer-2-2 .foot .no-items  |
| .section}                         | .section}                         |
| :::                               | :::                               |
+-----------------------------------+-----------------------------------+

::: {#footer-3 .foot .section name="Footer"}
::: {#Attribution1 .widget .Attribution data-version="1"}
::: {.widget-content style="text-align: center;"}
Simple theme. Powered by [Blogger](https://www.blogger.com).
:::

::: {.clear}
:::

[ [
[![](https://resources.blogblog.com/img/icon18_wrench_allbkg.png){width="18"
height="18"}](//www.blogger.com/rearrange?blogID=1453735247586098860&widgetType=Attribution&widgetId=Attribution1&action=editWidget&sectionId=footer-3 "Edit"){.quickedit}
]{.item-control .blog-admin} ]{.widget-item-control}

::: {.clear}
:::
:::
:::
:::
:::

::: {.footer-cap-bottom .cap-bottom}
::: {.cap-left}
:::

::: {.cap-right}
:::
:::
:::
:::
:::

::: {.content-cap-bottom .cap-bottom}
::: {.cap-left}
:::

::: {.cap-right}
:::
:::
:::
:::
