---
layout: post
title: "Personography, or, Thinking about People with TEI"
date: 2014-03-18
categories:
- "Digital Humanities"
- "Ancient Sports Text Repository & Atlas"
tags:
- "Knight Lab"
- StorymapJS
---

# Personography, or, Thinking about People with TEI

In creating the Ancient Sports Text Repository and Atlas (ASTRA), I spent a lot of time deciding how I should encode and store my data. The obvious choice for the texts was the TEI (Textual Encoding Initiative) XML markup schema. This standard, which has been in use since 1994, is used by a large number of libraries, museums, and digital humanities projects to represent texts in a machine readable format. TEI is a remarkably versatile format, capable of expressing all sorts of information beyond the text itself. Since TEI is the standard for many of the major text projects in Classical Studies (eg., Perseus, Papyri.info, etc.), it was a natural choice for encoding the small texts that make up the bulk of the project.

However, because I want to encode things other than texts, I was a little hesitant, especially when thinking about how to encode people. Part of the project includes compiling as comprehensive as possible a collection of people from antiquity who were connected with athletics. This includes persons both literary and mythical—especially since it is often impossible to determine in which of these categories a particular individual belongs. It would perhaps be easier to encode this data in a traditional relational database. This data would include columns with an identifier, a name field, a plausibility field (mythical, established), a brief biography, and perhaps an array to refer to other texts in the database. The problem with using a database of this sort is that it doesn't allow much flexibility. The data could be encoded in HTML to provide links to other content, but it would be preferable to have the data encoded in a standard XML format so the XSLT form can extract it automatically along with other content. So preferably we would want to encode our data in the same format as everything else.

Luckily, TEI does provide the tools for making this possible. TEI refers to the encoding of people as "personography". I generally cringe at neologisms, but this term is at least descriptive. A personography is simply a structured way of representing biographical data about a person. Unfortunately, there are not a lot of resources for how to structure a personography with TEI; a further complication includes the fact that biography in general is not particularly well theorized. So a large part of the effort has come in deciding how exactly to structure such a biography using TEI. In the process, I believe I have come up with a method that is not only applicable to others who wish to use personography in other applications. In what follows, I am going to talk a little bit about how personography in TEI works, then see how it has been used in other contexts, point out some of the shortcomings and pitfalls, and finally then see how it can be used effectively, using the ASTRA project as an exemplum. I assume familiarity with XML, and some experience with TEI will be helpful.

### TEI Personography

The TEI standard is incredibly complex: a printed version of the code would surely run over a thousand pages long. Also, because the schema is on the whole not strict in its nesting policies, it allows the author a large amount of leeway in how to structure a document. Here, I will describe the primary tools needed to construct a personography. As will become apparent quickly, there is much room for interpretation; I will give examples later on that demonstrate how the personography can be put in practice.

The primary tag for personography is the  tag, which is a member of the "namesdates" module of the schema for dealing with names, dates, and places. In addition to the standard range of attributes used in TEI, the  tag has three unique attributes: role, sex, and age. It is recommended that the age attribute be used to indicate an age group, such as "teenager" or "senior", as opposed to a integer value. The sex attribute should be encoded using "M" to indicated male, "F" for female, "O" for other, "N" for none or not applicable, or "U" for unknown. One can also use the ISO 5218:2004 standard for the Representation of Human Sexes, in which "0" is used for unknown, "1" for male, "2" for female, and "9" for not applicable; however, this standard is now considered to be outdated, so the former coding should be preferred. The role attribute is a bit trickier, but only because it employs a local vocabulary, which is to say, a set of terms that the encoder determines beforehand. For example, if we were describing an edited volume, we might have roles for "author" and "editor" to describe the authors of individual chapters and the editor(s) of the entire volume; if describing a military text, we might use the role attribute for expressing an individual's rank. Because of the flexibility of this attribute, it is worthwhile to have a descriptive vocabulary before encoding begins.

Such are the main attributes for the  tag. You probably notice that there is no attribute for some of the more typical descriptions for people such as "name" or "birthdate". That is because the main work of a personography occurs within the person tag. The  tag can include many of the tag modules in the [TEI documentation][1]. The most important of these modules to personography is the "namesdates" module, to which the  tag itself belongs. The tags in this module are as follows:  , , , , , , , , , , , , , , , , , and .Each of these tags has their own set of attributes that are relevant to their usage, so when constructing a personography one will probably have to consult the documentation repeatedly to see what the range of options allow.

The TEI documentation provides the following example of personography, using the Roman poet Ovid as their object of encoding:
```
<person xml:id="Ovi01" sex="1" role="poet">
  <persName xml:lang="en">Ovid</persName>
  <persName xml:lang="la">Publius Ovidius Naso</persName>
  <birth when="-0044-03-20"> 20 March 43 BC
  <placeName>
    <settlement type="city">Sulmona</settlement>
    <country key="IT">Italy</country>
  </placeName> </birth>
  <death notBefore="0017" notAfter="0018">17 or 18 AD
    <placeName>
      <settlement type="city">Tomis (Constanta)</settlement>
      <country key="RO">Romania</country>
    </placeName>
  </death>
</person>
```
As you can see, using primarily the tags in the "namesdates" module, one can give a pretty straightforward description of the poet within the  tag, In addition to the attributes discussed above, you will see that the person tag has the attribute xml:id, which is used to uniquely identify the reference in the project at large. The name is described in two ways between  tags: first, the typical English shorthand "Ovid", and second, the longer, full Latin name of "Publius Ovidius Maro". His birth is given using the appropriate tag, which contains an attribute for the date (@when) and the nested  element to describle where. After the  element, we see another element representing his death encoded in similar fashion. As you can see, the data expressed is minimal, but straightforward.

Such is how a Personography is constructed in the abstract. Information is given or subtracted based on the needs of the creators. Before going on to how it is put into practice, it is relevant to note how the person element is contained, as that largely dictates how the  element is used in any particular document. The  tag can be contained by three elements: , , and . The  tag is used primarily for nesting a series of  elements one after another. Participant Description () is an element that describes all the people involved in a particular document; perhaps the most salient example is to mention all the characters in a play, or perhaps all the contributors to a particular document. The  tag is for organizations: as organizations are composed of people, we would likely then expect a list of people who constitute the organization to appear within the tag.

### Examples of Personography

As I mentioned above, there are not many digital humanities projects that use personography extensively. The typical usage of the person tag seems to be as a short reference inline, which may or may not refer to a list of persons, usually found somewhere in the header element. It is not always apparent, either, when the authors have attempted to create a more structured personography beyond the standard practice of marking persons. It seems the only way to be sure that a project is compiling a personography is if they refer to the document as a personography, biography, biographical gazetteer, or some other such synonym. In what follows, I will discuss two examples of personography where the authors have self-consciously applied the title to their documents. By examining how others have constructed their documents, a set of best practices may become apparent.

The first example is the [The William F. Cody Archive][2], run by the Buffalo Bill Center of the West and the University of Nebraska, which catalogues the life, times, and documents of "Buffalo Bill" Cody. The primary goal of the archive is to give access to the multitude of newspaper clippings, correspondence, and video which give the viewer insight into this famous Wild West Hero. All documents are encoded in TEI, to varying degrees of specificity.

The authors of the site have compiled all the persons mentioned in the texts and placed them into a single XML document, which they refer to as a personography. Unlike the other documents in the archive, no link is provided for the xml version of the text, so one must manually enter the [XML personography's address][3] to access the file. After providing typical TEI header information—file description, title statement, authorship, publication information—the file begins with  and  tags before starting a list of persons with . There are three lists which the 56 separate entries are distributed amongst:empire, personal, and business, referring to the different spheres which one could place a person in relation to Buffalo Bill.

The entries themselves follow a formula: in fact, the authors have commented out the format which they wish to follow at the bottom of the text. Here is an example, the first entry of a person:
```
<person xml:id="bailey.j">
  <persName type="display" subtype="lcsh">Bailey, James Anthony, 1847-1906</persName>
  <persName>
    <surname>Bailey</surname>
    ,
    <forename type="first">James</forename>
    <forename type="middle" full="yes">Anthony</forename>
    <addName type="birth">McGinnes</addName>
  </persName>
  <birth when="1847">1847</birth>
  <death when="1906">1906</death>
  <sex>male</sex>
  <affiliation/>
  <occupation/>
  <floruit/>
  <nationality/>
  <education/>
  <residence/>
  <faith/>
  <note>
    James A. Bailey (1847-1906) was born James Anthony McGinnes in 1847. As a teenager
    McGinnes became an assistant to Frederic Augusta Bailey, a nephew of circus pioneer
    Hachaliah Bailey. McGinnes eventually changed his name to James Anthony Bailey. [...]
  </note>
</person>
```
The only attribute in the name tag here is the XML identifier, which allows the application to point resources pertaining to the person to this entry (and vice versa). There are several different options presented for the name element. The first indicates what name should be displayed as the title of the entry when displayed in a list, which is designated by use of the type attribute "display". The second is his full given name encoded with  and , where the middle and first forename are distinguished with type attributes. Interestingly, because this particular person changed his name at one point in his life, the authors have included an  element to indicate this change from his birth name. The name fileds are followed by birth, death, and sex elements, and then a series of closed tags indicate choices which the authors did not provide information for (the majority of the entries leave the same fields blank). The final element is a  tag. In this element, the authors have provided a brief (one paragraph) biographical sketch of the person. The narrative is similar to what one would find in an encyclopedia entry, with an emphasis on the person's relationship to Buffalo Bill. Following the note, the person element is closed, and another similar entry begins.

What is most striking to me about the choices the authors made in constructing this personography was their strict use of formatting and heavy reliance on the note tag. First, it is clear from the onset that the authors settled on a list of elements they wished to include in each entry and stuck to it, to the point of leaving in empty elements. This practice takes a lot of the cognitive burden off the encoder, as he simply has to enter in the data, with only a little extra effort when idiosyncrasies arise, as the case with a person who changed their name. However, the authors relied heavily on a prose exposition for each entry, to the detriment of other encodable data: for example, there are no place elements anywhere, though these are certainly known for the birth and death elements at least. The note itself could also have been encoded; this means that if there is some reference in the note to another encoded entity, there will be no way (at least easily) to point the reader to that entity within the site.

A second example I will discuss is the [Map of Early Modern London][4] (MoEML) project developed by the University of Victoria in Canada. MoEML provides visitors with an interactive tour of London in and around 1561 CE through use of the Agas map, an early plan of the city. In addition to providing location data, MoEML  has compiled an Encyclopedia to go along with the data, broken up into three sections: a placeography, a personography, and an orgography (having to do with organizations). There are actually [three different flavors of personography][5] on the site. The first two are Historical and Literary personographies, which I will briefly mention, and then a handful of full-length biographies.

The official personography of the site is the Historical and Literary personographies. As expected, the Historical deals with real people, and the Literary with fictional characters. [The entries][6] are ordered in a list, of which the following is an example:
```
<person xml:id="BACO1" sex="1">
  <persName type="hist">
    <reg>Bacon, Sir Nicholas</reg>
    <roleName>Sir</roleName>
    <forename>Nicholas</forename>
    <surname>Bacon</surname>
  </persName>
  <birth when-custom="1510" datingMethod="includes.xml#julian" notBefore="1510-04-04" notAfter="1511-04-03"/>
  <death when-custom="1579" datingMethod="includes.xml#julian" notBefore="1579-04-04" notAfter="1580-04-03"/>
  <note>
    <p>Lawyer and administrator.</p>
    <list type="links">
      <item>
        <ref target="http://www.oxforddnb.com/view/article/1002">
          <title level="m">ODNB</title>
        </ref>
      </item>
    </list>
  </note>
</person>
```
Here, the  tag contains an XML id and sex attributes. The name is fully expressed in the  element, with the special tags  for express the accepted name and  to indicate a title. The birth and death tags just give the date, although the attributes are somewhat complicated. The authors of this site have included the @datingMethod attribute to help indicate that this particular entry is to be handled using the Julian calendar. Because the time frame which the map covers spans across the Julian and Gregorian calendars, so the dates differ depending on whether the source uses one calendar or the other. The authors thus establish which calendar their source was using and encode the date to reflect it. Coding is performed on the back end to make sure that the calendars coincide with one another for timeline purposes. Following the date elements, the authors have included a  element to give further details about the individual. Rather than create their own bibliography, as in the Cody Archive, MoEML gives just a headline description of the person and links to external content—in this case, an Oxford Dictionary of National Biography entry.

This entry is on the whole more compact than that of the Cody Archive, but MoEML also has a series of experimental personographies that have expansive entries on important personages. These personographies are full-fledged biographies, several paragraphs long and with plenty of scholarly references (too long to provide a concise example). This collection is an offshoot from the main content of the site, written by graduate students who participated in the site's development. They are also encoded using TEI, but take a different tact than the two examples given above. Instead of encoding the information using the  element, they give the biography using  and  tags. In other words, the document is envisioned as a scholarly work in its own right and less as a collection of biographical statistics. There are also separate TEI headers for each entry, complete with file and revision descriptions; bibliographical citations are referenced in a separate site-wide XML bibliography. The text has two major sections: a  section that contains a  element with the name of the person, and the main  section with the content (under a
 element) separated into paragraphs with a basic
 tag. In each paragraph appears the text of the biography. Title, people, and bibliography citations are tagged with references to the appropriate resources.

An interesting omission to the texts is that basic biographical information–such as date of birth, place of birth, death, nationality, etc.–are not encoded using XML tags. Some of this information is given the body of the text through narrative, but it does not receive special encoding. The downside of this approach is that it does not provide a machine parsable way to extract this information, and should the reader wish to, say, pull out all the entries for people alive in a particular decade, they would have to read each entry themselves and compile a list themselves, rather than having a search application find the list automatically.

With these three examples, we see three different strategies for approaching the coding of persons using TEI XML. The first two use the  tag, are part of documents containing long lists of people, and contain only a small amount of information about the person. The MoEML personography has extremely short entries, outsourcing a large part of information as links to resources hosted on external sites. The Cody Archive's personography has more information, included using a note tag, and other information as pertinent to their site's content. The final example from MoEML, on the other hand, are much longer and encoded using  and  tags. They provide detailed information about the person, but much of the typical biograhpical information is not encoded for convenient machine reading. All strategies have their plusses and minuses, but are designed as appropriate for their diverse purposes.

### Personography for the ASTRA Project

The examples given above suggest several ways to think about personography, but the greatest lesson to be learned is that a personography must fit appropriately the scope of the project. The ASTRA project separates its inforrmation into several "buckets": texts, people, events, sports, artifacts, and scholarship. For the project to function effectively, the items in the different buckets need to have a way to communicate with one another. That means each text will ideally provide links that will link the data in the site, both internally and externally.

By comparing the different examples of personography against our needs, a clearer picture of how our personography should be structured emerges. First, the MoEML and Cody Archive personographies are too short and not specialized enough for our purposes. Neither would be appropriate for the linking of events and sports, though the MoEML personography does hint at an appropriate way of linking to scholarship and texts. Another consideration: given that there will likely be a good number of entries in the database, it seems conceptually more appropriate to give each entry its own XML document. Computationally, this should not be problematic, and should provide some ease of access when editing particular entries. The longer biographies at MoEML have more of the type of information we would like to include. The tagging of description content also seems appropriate. However, the lack of encoding of biographical data would have to be included and the descriptions are rather too long–this can be included with strategic referencing of information.

With these considerations in mind, it becomes apparent that we need to find some middle ground between these styles. After some tinkering, here is an example of a TEI personography for the ASTRA project for the first recorded victor of the Olympic Games, Coroebus of Elis:
```
  <TEI xmlns="http://www.tei-c.org/ns/1.0" xml:id="koroibos1">
  <teiHeader>
    <revisionDesc>
      [...]
    </revisionDesc>
  </teiHeader>
  <person type="hist" role="victor" sex="M">
    <persName xml:lang="en">Coroebus</persName>
    <persName xml:lang="de">Koroibos</persName>
    <persName xml:lang="grc">Κόροιβος</persName>
    <!-- *** DATING *** -->
    <birth source="paus0508">
      <placeName type="polis" ref="570220">Elis</placeName>
    </birth>
    <floruit when="-0776"/>
    <death source="paus0508 paus0826">Tomb located at the Eastern edge of
      <placeName type="polis" ref="570220">Elis</placeName> and was used in defining territorial
      boundaries.</death>
    <!-- *** SPORT CONTENT: Biographical Sketch & Events *** -->
    <note>Reputedly the winner of the Stade in the first Olympic Games.</note>
    <event type ="stadion" subtype="victory" when="-0776" source="paus0826">
      <placeName ref="570531">Olympia</placeName>
    </event>
    <!-- *** BIBLIOGRAPHY *** -->
    <listBibl>
      <bibl type="primary" ref="ath1">Athenaeus, 9.382B</bibl>
      <bibl type="primary" ref="paus0508">Pausanias, 5.8.5-6</bibl>
      <bibl type="primary" ref="paus0826">Pausanias, 8.26.3-4</bibl>
      <bibl type="primary" ref="strabo080330">Strabo, 8.30.3</bibl>
      <bibl type="Moretti">1</bibl>
    <listBibl>
  </person>
</TEI>
```
As you can see, the document begins with the  element as the parent element with the appropriate xml:id. Because of the many xml ids, these are kept in a separate spreadsheet to ensure there is no overlap.  This is followed by a TEI header, which provides file and revision information (omitted here for brevity). The main document information is contained in a  tag. The person element is broken into four sections: name, dating, sports content, and bibliography.

The  tag itself provides several peices of information. First, it specifies that this person is a historical person, as opposed to a mythical person or a scholar. The role is defined as victor, insofar as he was a victor in an Olympic games. This seems an appropriate category, as it is often an important distinction in scholarship. The sex attribute is straightforward. The name section also provides three different ways of representing a person's name: the anglicized version, the bare transliteration or "continental" version, and the Greek original text.

This is followed by dating and location data. The place and date are sourced, which is to say, they refer through XML refs to texts on the site which provide the information given. The information also provides text descriptions where interesting or necessary. The floruit is included as the date he was active. Note that the references for place reference Pleiades locations, a convenient reference from a trusted source.

The sports content is the meat of the article. It begins with a note, that gives a brief overview of who the person was and what his importance to the project is (there isn't a lot of information Coroebus, so this section isn't very long). The next element is an  tag. This is an imaginative use of the tag, slightly different from its intended use, but corresponds to a particular event in the Olympic Games. It gives all the information about the staging of the event, including the where and when. The type of event is an attribute (the stadion is a sprint), and his place as victor and the source of the information are also given in that form. For other entries, where the athlete has multiple victories, there would be multiple event elements in this section.

The final section gives bibliographic information. The first entries are primary sources, which would be contained as part of the text repository and linked via their xml refs. This is followed by a link to standard reference work, Moretti's list of Olympic victors.

As much as possible, elements will be linked to other documents on the site. XSLT spreadsheets will be used to pull information and display them in a standardized format. The information will also lik to external sites where appropriate. The scope is large, but it is hoped this project will not only add to scholarship on ancient sports, but also provide a new perspective on how to structure personographies and biographies in general.

### Resources for Personography

[1]: http://www.tei-c.org/release/doc/tei-p5-doc/en/html/ref-person.html "TEI element person"
[2]: http://codyarchive.org "The William F. Cody Archive"
[3]: codyarchive.org/life/wfc.person.xml "Cody Archive personography"
[4]: http://mapoflondon.uvic.ca "MoEML"
[5]: http://mapoflondon.uvic.ca/mdtEncyclopediaPersonography.htm?listType=subcategory "MoEML Personographies"
[6]: http://mapoflondon.uvic.ca/historical_personography.xml "MoEML Personography"
