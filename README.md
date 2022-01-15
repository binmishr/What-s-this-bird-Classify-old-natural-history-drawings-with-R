# What-s-this-bird-Classify-old-natural-history-drawings-with-R

The details of the codeset and plots are included in the attached Microsoft Word Document (.docx) file in this repository. 
You need to view the file in "Read Mode" to see the contents properly after downloading the same.

The following R packages have been utilized on this repository and attached the code as .ZIP file:

    i)   spelling
    ii)  hocr
    iii) cld3
    iv)  cld2
    v)   tesseract
    vi)  magick
    vii) taxize
    
  ClD2 Package - A Brief Introduction
  ====================================
  
CLD2 probabilistically detects over 80 languages in Unicode UTF-8 text, either plain text or HTML/XML. For mixed-language input, CLD2 returns the top three languages found and their approximate percentages of the total text bytes (e.g. 80% English and 20% French out of 1000 bytes)

Installation
=============
This package includes a bundled version of libcld2:

        Library("cld2")
        cld2::detect_language("To be or not to be")

# [1] "ENGLISH"

        cld2::detect_language("Ce n'est pas grave.")

# [1] "FRENCH"

        cld2::detect_language("Nou breekt mijn klomp!")

# [1] "DUTCH"

        Set plain_text = FALSE if your input contains HTML:

        cld2::detect_language(url('http://www.un.org/ar/universal-declaration-human-rights/'), plain_text = FALSE)

# [1] "ARABIC"

        cld2::detect_language(url('http://www.un.org/zh/universal-declaration-human-rights/'), plain_text = FALSE)

# [1] "CHINESE"

        Use detect_language_multi() to get detailed classification output.

        detect_language_multi(url('http://www.un.org/fr/universal-declaration-human-rights/'), plain_text = FALSE)

# $classification
        #   language code latin proportion
        # 1   FRENCH   fr  TRUE       0.96
        # 2  ENGLISH   en  TRUE       0.03
        # 3   ARABIC   ar FALSE       0.00
# 
# $bytes
        # [1] 17008
# 
# $reliabale
# [1] TRUE
This shows the top 3 language guesses and the proportion of text that was classified as this language. The bytes attribute shows the total number of text bytes that was classified, and reliable is a complex calculation on if the #1 language is some amount more probable then the second-best Language.

CLD3 Package - A Brief Introduction
===================================

Google's Compact Language Detector 3 is a neural network model for language identification and the successor of CLD2 (available from) CRAN. This version is still experimental and uses a novell algorithm with different properties and outcomes. 

Example
========
The function detect_language() is vectorised and guesses the the language of each string in text or returns NA if the language could not reliably be determined.

        > library(cld3)
        > example(cld3)

        cld3> # Vectorized best guess
        cld3> detect_language(c("To be or not to be?", "Ce n'est pas grave.", "猿も木から落ちる"))
        [1] "en" "fr" "ja"
        The function detect_language_multi() is not vectorised and detects all languages inside the entire character vector as a whole.

        cld3> # Multiple languages in one text
        cld3> detect_language_mixed("This piece of text is in English. Този текст е на Български.", size = 3)
          language probability reliable proportion
        1       bg   0.9173891     TRUE  0.5853658
        2       en   0.9999790     TRUE  0.4146341
        3      und   0.0000000    FALSE  0.0000000
Installation
==============
Binary packages for OS-X or Windows can be installed directly from CRAN:

install.packages("cld3")

Installation from source on Linux or OSX requires Google's Protocol Buffers library. On Debian or Ubuntu install libprotobuf-dev and protobuf-compiler:

        sudo apt-get install -y libprotobuf-dev protobuf-compiler
On Fedora we need protobuf-devel:

        sudo yum install protobuf-devel
On CentOS / RHEL we install protobuf-devel via EPEL:

        sudo yum install epel-release
        sudo yum install protobuf-devel
On OS-X use protobuf from Homebrew:

        brew install protobuf
        
Tesseract Package - A Brief Introduction
=========================================

Bindings to Tesseract-OCR: a powerful optical character recognition (OCR) engine that supports over 100 languages. The engine is highly configurable in order to tune the detection algorithms and obtain the best possible results. Project Status: Active – The project has reached a stable, usable state and is being actively developed. CRAN_Status_Badge CRAN RStudio mirror downloads

        Upstream Tesseract-OCR documentation: https://tesseract-ocr.github.io/tessdoc/
        Introduction: https://docs.ropensci.org/tesseract/articles/intro.html
        Reference: https://docs.ropensci.org/tesseract/reference/ocr.html

        Hello World
        Simple example

# Simple example
        text <- ocr("https://jeroen.github.io/images/testocr.png")
        cat(text)

# Get XML HOCR output
        xml <- ocr("https://jeroen.github.io/images/testocr.png", HOCR = TRUE)
        cat(xml)
        Roundtrip test: render PDF to image and OCR it back to text

# Full roundtrip test: render PDF to image and OCR it back to text
        curl::curl_download("https://cran.r-project.org/doc/manuals/r-release/R-intro.pdf", "R-intro.pdf")
        orig <- pdftools::pdf_text("R-intro.pdf")[1]

# Render pdf to png image
        img_file <- pdftools::pdf_convert("R-intro.pdf", format = 'tiff', pages = 1, dpi = 400)

# Extract text from png image
        text <- ocr(img_file)
        unlink(img_file)
        cat(text)
        
Installation
=============
On Windows and MacOS the package binary package can be installed from CRAN:

        install.packages("tesseract")

Install from source On Debian or Ubuntu install libtesseract-dev and libleptonica-dev. Also install tesseract-ocr-eng to run examples.

        sudo apt-get install -y libtesseract-dev libleptonica-dev tesseract-ocr-eng
On Ubuntu you can optionally use this PPA to get the latest version of Tesseract:

        sudo add-apt-repository ppa:alex-p/tesseract-ocr-devel
        sudo apt-get install -y libtesseract-dev tesseract-ocr-eng
On Fedora we need tesseract-devel and leptonica-devel

        sudo yum install tesseract-devel leptonica-devel
On RHEL and CentOS we need tesseract-devel and leptonica-devel from EPEL

        sudo yum install epel-release
        sudo yum install tesseract-devel leptonica-devel
On OS-X use tesseract from Homebrew:

        brew install tesseract
        
Tesseract uses training data to perform OCR. Most systems default to English training data. To improve OCR results for other languages you can to install the appropriate training data. On Windows and OSX you can do this in R using tesseract_download():

        tesseract_download('fra')
On Linux you need to install the appropriate training data from your distribution. For example to install the spanish training data:

        tesseract-ocr-spa (Debian, Ubuntu)
        tesseract-langpack-spa (Fedora, EPEL)
        
Magick Package - A Brief Introduction
======================================

Bindings to ImageMagick: the most comprehensive open-source image processing library available. Supports many common formats (png, jpeg, tiff, pdf, etc) and manipulations (rotate, scale, crop, trim, flip, blur, etc). All operations are vectorized via the Magick++ STL meaning they operate either on a single frame or a series of frames for working with layers, collages, or animation. In RStudio images are automatically previewed when printed to the console, resulting in an interactive editing environment.

        library(magick)
        image_trim(frink)
        image_scale(frink, "200x200")
        image_flip(frink)
        image_rotate(frink, 45) ## <-- result of this is shown
        image_negate(frink)
        frink %>% 
          image_background("green") %>% 
          image_flatten() %>%
          image_border("red", "10x10")
        image_rotate(frink, 45) %>% image_write("man/figures/frink-rotated.png")


Effects
=======
        image_oilpaint(frink)
        image_implode(frink)
        image_charcoal(frink) ## <-- result of this is shown
        image_blur(frink)
        image_edge(frink)
        image_charcoal(frink) %>% image_write("man/figures/frink-charcoal.png")


Create GIF animation:

# Download images
        oldlogo <- image_read("https://developer.r-project.org/Logo/Rlogo-2.png")
        newlogo <- image_read("https://jeroen.github.io/images/Rlogo-old.png")
        logos <- c(oldlogo, newlogo)
        logos <- image_scale(logos, "400x400")

# Create GIF
        (animation1 <- image_animate(logos))
        image_write(animation1, "man/figures/anim1.gif")

# Morph effect  <-- result of this is shown
        (animation2 <- image_animate(image_morph(logos, frames = 10)))
        image_write(animation2, "man/figures/anim2.gif")


Read GIF animation frames. See the rotating earth example GIF:

        earth <- image_read("https://upload.wikimedia.org/wikipedia/commons/2/2c/Rotating_earth_%28large%29.gif")
        length(earth)
        earth[1]
        earth[1:3]
        earth1 <- rev(image_flip(earth)) ## How Austrialans see earth
        image_write(earth1, "man/figures/earth1.gif") ## <-- result of this is shown


R logo with dancing banana:

        logo <- image_read("https://www.r-project.org/logo/Rlogo.png")
        front <- image_scale(banana, "300")
        background <- image_scale(logo, "400")
        frames <- lapply(as.list(front), function(x) image_flatten(c(background, x)))
        image_write(image_animate(image_join(frames)), "man/figures/Rlogo-banana.gif")

Installation
=============
Binary packages for macOS or Windows can be installed directly from CRAN:

        install.packages("magick")
Installation from source on Linux or OSX requires the imagemagick Magick++ library. On Debian or Ubuntu install libmagick++-dev:

        sudo apt-get install -y libmagick++-dev
If you are on Ubuntu 14.04 (trusty) or 16.04 (xenial) you can get a more recent backport from the ppa:

        sudo add-apt-repository -y ppa:cran/imagemagick
        sudo apt-get update
        sudo apt-get install -y libmagick++-dev 
On Fedora, CentOS or RHEL we need ImageMagick-c++-devel. However on CentOS the system version of ImageMagick is quite old. More recent versions are available from the ImageMagick downloads website.

        sudo yum install ImageMagick-c++-devel
On macOS use imagemagick@6 from Homebrew.

        brew install imagemagick@6
The unversioned homebrew formulaimagemagick can also be used, however it has some unsolved OpenMP problems.

HOCR Package - A Brief Introduction
====================================
The goal of hocr is to facilitate post-OCR data processing and wrangling. The package exposes hocr parcer, hocr_parse, which converts XHTML format output into tidy tibble with one word per row. In addition to the columns exported by tesseract::ocr_data, hocr outputs additional metadata regarding organization of words into lines, paragraphs, content areas and pages. Read more about hOCR specification here.One of the key elements of hocr format is “bounding box” - a rectangular region of the image covering the extent of the word recognized by tesseract. This bbox can be used to extract respective part of the image using, for example magick package, using bbox_to_geometry helper function.hocr aslo includes tidiers for common hOCR-capable systems. As of version 0.0.9000 only tesseract output format is supported, but in the future, support for OCRopus will be added.

Installation
=============
You can install the development version from GitHub with:

Library("hocr")

Example
=======
This is a basic example which shows you how to solve a common problem:

        library(hocr)
        library(tesseract) # OCR
        library(tidyverse) # data wrangling and viz
        #devtools::install_github("thomasp85/patchwork")
        library(patchwork) # arranging plots
        We will OCR a page from an old cookbook retrieved from archive.org[1] and enhanced using magick package (see image preparation script on github).

        cupcakes <- system.file("extdata", "peanutbutter.png", package="hocr")


        recipe <- tesseract::ocr(cupcakes, HOCR = TRUE) %>% 
          hocr::hocr_parse() %>% 
          hocr::tidy_tesseract()
          
        recipe
        #> # A tibble: 234 x 21
        #>    ocrx_word_id ocrx_word_bbox ocrx_word_conf ocrx_word_tag ocrx_word_value
        #>    <chr>        <chr>                   <dbl> <chr>         <chr>          
        #>  1 word_1_1     38 58 271 103              85 strong        Chocolate      
        #>  2 word_1_2     287 61 451 103             86 text          Peanut         
        #>  3 word_1_3     468 62 619 103             89 text          Butter         
        #>  4 word_1_4     636 60 852 113             84 strong        Cupcakes       
        #>  5 word_1_5     36 153 112 182             87 strong        Your           
        #>  6 word_1_6     123 152 184 1~             88 strong        kids           
        #>  7 word_1_7     196 152 250 1~             88 strong        will           
        #>  8 word_1_8     264 152 324 1~             85 strong        love           
        #>  9 word_1_9     337 152 417 1~             84 strong        these          
        #> 10 word_1_10    431 154 472 1~             90 text          (as            
        #> # ... with 224 more rows, and 16 more variables: ocr_line_id <chr>,
        #> #   ocr_line_bbox <chr>, ocr_line_xbaseline <dbl>,
        #> #   ocr_line_ybaseline <dbl>, ocr_line_xsize <dbl>,
        #> #   ocr_line_xdescenders <dbl>, ocr_line_xascenders <dbl>,
        #> #   ocr_par_id <chr>, ocr_par_lang <chr>, ocr_par_bbox <chr>,
        #> #   ocr_carea_id <chr>, ocr_carea_bbox <chr>, ocr_page_id <chr>,
        #> #   ocr_page_image <chr>, ocr_page_bbox <chr>, ocr_page_no <dbl>

Now that data is in the tidy format, lets render the page in ggplot and identify bounding boxes around words and paragraphs to illustrate the benefits of parsed document structure. tesseract outputs bboxes in upper-left corner coordinate system. We will transform all y-values to bottom-left scale and plot the bounding boxes alongside with the original picture, colored by tesseract confidence score.

        p1 <- recipe %>% 
          mutate(ocrx_word_bbox=lapply(ocrx_word_bbox, function(x) 
            separate(as_tibble(x), value, into=c("word_x1", "word_y1", "word_x2", "word_y2"), convert = TRUE))) %>% 
            unnest(ocrx_word_bbox) %>% 
          mutate(ocr_page_bbox=lapply(ocr_page_bbox, function(x) 
            separate(as_tibble(x), value, into=c("page_x1", "page_y1", "page_x2", "page_y2"), convert = TRUE))) %>% 
            unnest(ocr_page_bbox) %>% 
          mutate(word_y1=max(page_y2)-word_y1,
                 word_y2=max(page_y2)-word_y2) %>% 
            ggplot(aes(xmin=word_x1, ymin=word_y1, xmax=word_x2, ymax=word_y2))+
            geom_rect(aes(color=ocr_par_id, fill=ocrx_word_conf), show.legend = TRUE)+
          theme_minimal()+
          theme(panel.grid = element_blank(), 
                axis.text = element_text(size = 7), 
                legend.text = element_text(size = 7), 
                legend.title = element_text(size = 7))

        library(png)
        library(grid)
        img <- readPNG(cupcakes)
        p2 <- rasterGrob(img, interpolate=TRUE)

        p1+p2

![image](https://user-images.githubusercontent.com/26252963/149612952-006e4f75-84a5-4eb5-924a-6bdac15daf17.png)

There is also a fork of imagemagick called graphicsmagick, but this doesn't work for this package.

Taxizedb Package - A Brief Introduction
========================================
taxizedb - Tools for Working with Taxonomic Databases on your machine.Docs: https://docs.ropensci.org/taxizedb/

taxize is a heavily used taxonomic toolbelt package in R - However, it makes web requests for nearly all methods. That is fine for most cases, but when the user has many, many names it is much more efficient to do requests to a local SQL database.

Data sources

Not all taxonomic databases are publicly available, or possible to mash into a SQLized version. Taxonomic DB's supported:

        NCBI: text files are provided by NCBI, which we stitch into a sqlite db
        ITIS: they provide a sqlite dump, which we use here
        The PlantList: created from stitching together csv files. this source is no longer updated as far as we can tell. they say they've moved focus to the World Flora Online
        Catalogue of Life: created from Darwin Core Archive dump.
        GBIF: created from Darwin Core Archive dump. right now we only have the taxonomy table (called gbif), but will add the other tables in the darwin core archive later
        Wikidata: aggregated taxonomy of Open Tree of Life, GLoBI and Wikidata. On Zenodo, created by Joritt Poelen of GLOBI.
        World Flora Online: http://www.worldfloraonline.org/
        
Update schedule for databases:

        NCBI: since db_download_ncbi creates the database when the function is called, it's updated whenever you run the function
        ITIS: since ITIS provides the sqlite database as a download, you can delete the old file and run db_download_itis to get a new dump; 
        they I think update the dumps every month or so
        The PlantList: no longer updated, so you shouldn't need to download this after the first download. hosted on Amazon S3
        Catalogue of Life: a GitHub Actions job runs once a day at 00:00 UTC, building the lastest COL data into a SQLite database thats hosted on Amazon S3
        GBIF: a GitHub Actions job runs once a day at 00:00 UTC, building the lastest GBIF data into a SQLite database thats hosted on Amazon S3
        Wikidata: last updated April 6, 2018. Scripts are available to update the data if you prefer to do it yourself.
        World Flora Online: since db_download_wfo creates the database when the function is called, it's updated whenever you run the function
Links:

        NCBI: ftp://ftp.ncbi.nih.gov/pub/taxonomy/
        ITIS: https://www.itis.gov/downloads/index.html
        The PlantList - http://www.theplantlist.org/
        Catalogue of Life:
        latest monthly edition via http://www.catalogueoflife.org/DCA_Export/archive.php
        GBIF: http://rs.gbif.org/datasets/backbone/
        Wikidata: https://zenodo.org/record/1213477
        World Flora Online: http://www.worldfloraonline.org/
       
All databases are SQLite.

Package API
This package for each data sources performs the following tasks:

        Downloaded taxonomic databases db_download_*
        Create dplyr SQL backend via dbplyr::src_dbi - src_*
        Query and get data back into a data.frame - sql_collect
        Manage cached database files - tdb_cache
        Retrieve immediate descendents of a taxon - children
        Retrieve the taxonomic hierarchies from local database - classification
        Retrieve all taxa descending from a vector of taxa - downstream
        Convert species names to taxon IDs - name2taxid
        Convert taxon IDs to species names - taxid2name
        Convert taxon IDs to ranks - taxid2rank
        
You can use the src connections with dplyr, etc. to do operations downstream. Or use the database connection to do raw SQL queries.

install
========
cran version

        install.packages("taxizedb")
        
Spelling Package - A Brief Introduction
========================================

Spell checking common document formats including latex, markdown, manual pages, and description files. Includes utilities to automate checking of documentation and vignettes as a unit test during 'R CMD check'. Both British and American English are supported out of the box and other languages can be added. In addition, packages may define a 'wordlist' to allow custom terminology without having to abuse punctuation.

Spell Check Single Files
=========================
The function spell_check_files automatically parses known text formats and only spell checks text blocks, not code chunks.

spell_check_files('README.md', lang = 'en_US')
        #   WORD       FOUND IN
        # AppVeyor   README.md:5
        # CMD        README.md:12
        # RStudio    README.md:8
For more information about the underlying spelling engine and how to add support for other languages, see the hunspell package.

![image](https://user-images.githubusercontent.com/26252963/149613150-77411b1f-5a42-426f-9617-a35620fd7d78.png)


Spell Check a Package
Spell check documentation, description, readme, and vignettes of a package:

        spell_check_package("~/workspace/V8")
        # DESCRIPTION does not contain 'Language' field. Defaulting to 'en-US'.
        #   WORD          FOUND IN
        # ECMA          V8.Rd:16, description:2,4
        # emscripten    description:5
        # htmlwidgets   JS.Rd:16
        # JSON          V8.Rd:33,38,39,57,58,59,121
        # jsonlite      V8.Rd:42
        # Ooms          V8.Rd:41,121
        # th            description:3
        # Xie           JS.Rd:26
        # Yihui         JS.Rd:26
Review these words and then update the wordlist to allow them:

        update_wordlist("~/workspace/V8")
        # The following words will be added to the wordlist:
        #  - ECMA
        #  - emscripten
        #  - htmlwidgets
        #  - JSON
        #  - jsonlite
        #  - Ooms
        #  - th
        #  - Xie
        #  - Yihui
        # Are you sure you want to update the wordlist?
        # 1: Yes
        # 2: No
Then these will no longer be marked as errors:

        > spell_check_package("~/workspace/V8")
        No spelling errors found.
        
Automate Package Spell Checking
===============================
Use spell_check_setup() to add a unit test to your package which automatically runs a spell check on documentation and vignettes during R CMD check if the environment variable NOT_CRAN is set to TRUE. By default this unit test never fails; it merely prints potential spelling errors to the console.

        spell_check_setup("~/workspace/V8")
        # Adding 'Language: en-US' to DESCRIPTION
        # No changes required to /Users/jeroen/workspace/V8/inst/WORDLIST
        # Updated /Users/jeroen/workspace/V8/tests/spelling.R
        
Note that the NOT_CRAN variable is automatically set to 1 on Travis and in devtools or RStudio, otherwise you need to set it yourself:

        export NOT_CRAN=1
        R CMD check V8_1.5.9000.tar.gz
        # * using log directory ‘/Users/jeroen/workspace/V8.Rcheck’
        # * using R version 3.5.1 (2018-07-02)
        # * using platform: x86_64-apple-darwin15.6.0 (64-bit)
        # ...
        # ...
        # * checking tests ...
        #   Running ‘spelling.R’
        #   Comparing ‘spelling.Rout’ to ‘spelling.Rout.save’ ... OK
        #   Running ‘testthat.R’
        #  OK
