---
layout: post
title:  "PDF Thumbnail Generator - OSX"
date:   2009-03-15
categories: PDF Thumbnail osx softwaredevelopment
---
Download: [PDF Thumbnail Generator - OSX.zip](https://www.andrewrondeau.com/Writings/PDF%20Thumbnail%20Generator%20-%20OSX.zip)

From Readme.txt

    PDF Thumbnail Generator for OSX
    (Developed and tested on Leopard)
    
    Version 1.0
    March 15, 2009
    
    Andrew Rondeau
    http://andrewrondeau.com
    
    All contents of this zip file are freely copyable.  For more information,
    please contact me at http://andrewrondeau.com
    
    --------------
    
    About:
    
    The PDF Thumbnail Generator is designed to allow for easy creation of
    thumbnails from PDF files.
    
    There are two versions:
    
     - Create Thumbnails of the First Page of Many PDFs:
         Creates a thumbnail of the first page of every PDF that is selected
         when the workflow is run
    
     - Create Thumbnails of Every Page of a Single PDF:
         Creates a thumbnail of every page of a PDF, allows for interactive
         saving of desired pages.
    
    To run, select the Workflow application with the robot icon.  The document
    icon will open the source in Automator if you choose to edit the
    workflow.
    
    
    --------------
    
    Create Thumbnails of the First Page of Many PDFs:
    
    When run, you will be able to select multiple PDF files.  You will then be
    prompted for some options; such as image size and file type.  The thumbnails
    will be saved in the same folder as the source PDF files, with the name
    "Copy of xxx.jpeg"
    
    The thumbnails will always be the first page of each PDF file.
    
    This workflow might also work with other kinds of files, although it was
    only tested with PDFs.  I will only support this script with PDF files.
    
    
    --------------
    
    Create Thumbnails of Every Page of a Single PDF
    
    Sometimes, the thumbnails automatically generated from the first page of a
    PDF file aren't what's needed.  In some instances, the best page for a
    thumbnail isn't the first page.  Other times, the page needs rotation before
    being saved as a thumbnail.
    
    This workflow is designed for when more care must be taken with generating
    a thumbnail.
    
    When the workflow is run, you will be able to select a single PDF file.
    You will then be prompted for many options, such as image type, size, and
    compression quality.
    
    Some steps might take a few seconds.  If you have a large PDF, you might
    have to wait a minute or two.
    
    Once resizing is complete, the PDF will open in four different Preview
    windows.  Each window will have a different rotation of the PDF, either 0,
    90, 180, or 270 degrees.  The pages will be out-of-order.  In Preview, you
    will need to select the page(s) that you want to save, and then navigate
    to File -> Save As.
