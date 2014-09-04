---
layout: post
title: Groovy - Nix style 'tree'
---
![](/img/posts/nix-style-tree/tree.jpg)

A little groovy script I added to output a tree of files (see image, nix 'tree' on left, this script on right). Originally did it in a gradle script but decided to remove the gradle specific stuff (e.g. file()) so it can be used anywhere in groovy:

    def printTree(path, lastFolderTrace){
      if (lastFolderTrace.empty)println path
      def files = new File(path).listFiles()
      files.each(){
        lastFolderTrace.each{ printFolder ->
          print printFolder ? "    " : "|   "
        }
        println  it == files.last()  ? "`-- $it.name" : "|-- $it.name"
        if (it.directory){
            printTree(it.path, lastFolderTrace +  (it == files.last()) )
        }
      }
    }

It's recursive and is invoked by:

    printTree("/home/phil", [])