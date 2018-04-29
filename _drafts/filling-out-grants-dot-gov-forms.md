

# Filling out Grants.gov forms 

## Background 

### Intent 

Automate form filling protion of the grant application process. 

### History 

Until recently, forms were filled out via *pdf packages*. Now, that method is being phased out for the *workspace* system. 

A *workspace* is a new system for grant applications. 

### On the nature of pdfs 

#### Structure

See [this gnu doc](https://web.archive.org/web/20141010035745/http://gnupdf.org/Introduction_to_PDF) for an introduction. 

See [wikipedia](https://en.wikipedia.org/wiki/Portable_Document_Format) for a better one. 

#### Concering Forms 

Forms are [Interactive Elements](https://en.wikipedia.org/wiki/Portable_Document_Format#Interactive_elements) 

The old common *form* for forms was [XFA](https://en.wikipedia.org/wiki/XFA) (*XML Forms Architecture*). See [here](https://www.w3.org/1999/05/XFA/xfa-template.html) for a 1999 specification. 

The current common *form* for forms is Acroforms. Specification is in section 8.6 *Interactive Deatures > Interactive Forms* of Adobe's [PDF Reference](http://www.adobe.com/devnet/pdf/pdf_reference.html). The *actual* [iso reference](https://www.iso.org/standard/51502.html) about a dollar. 


## Implementation 

### Sources 

* Stack Overflow 
    * [SO Question on editing pdfs in ruby](https://stackoverflow.com/questions/9185942/how-to-edit-or-write-on-existing-pdf-with-ruby)
    * [SO on filling out forms with ruby](https://stackoverflow.com/questions/33621861/fill-pdf-form-using-ruby)
    * [SO form automation in java](https://stackoverflow.com/questions/11701732/how-to-automate-pdf-form-filling-in-java)
* Tutorials 
    - [using pdf-forms](http://adamalbrecht.com/2014/01/31/pre-filling-pdf-form-templates-in-ruby-on-rails-with-pdftk/)
    - [using itext](http://blogs.thewehners.net/josh/posts/406-using-itext-to-generate-pdfs-in-rails-jruby-vs-ruby-java-bridge)

### Similar Projects 

* [District Housing](https://github.com/codefordc/districthousing)
    - Rails app for filling out section 8 housing application forms. 
    - uses pdf-forms
    - Frontend in bootstrap 

### Tooling 

* [iText](https://itextpdf.com)
    - [docs](http://itextsupport.com/apidocs/itext7/latest/)
    - [Example](https://developers.itextpdf.com/examples/form-examples/pdfxfa-examples#2767-removexfa.java) for form filling. 
    - [liscensing](https://developers.itextpdf.com/question/can-i-use-itext-without-respecting-agpl-license)
    - [OpenPDF](https://github.com/LibrePDF/OpenPDF/) is a fork of iText, easier liscencing 
* [pdftk](https://www.pdflabs.com/tools/pdftk-the-pdf-toolkit/)
    - It itself, is a frontend to [iText](https://itextpdf.com). 
    - The tool that *just keeps* popping up. 
    - Open source, but commercial use requires a liscense. 
* [pdf-forms](https://github.com/jkraemer/pdf-forms)
    - Ruby gem wrapper for pdftk.


## Form List 

* Application for Federal Assistance SF-424
* Budget Narrative Attachment Form
* Project Narrative Attachment Form
* Budget Information for Non-Construction Programs (SF-424A)
* Assurances for Non-Construction Programs (SF-424B)