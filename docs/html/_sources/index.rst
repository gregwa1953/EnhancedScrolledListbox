.. The Enhanced Listbox documentation master file, created by
   sphinx-quickstart on Thu Feb 13 04:43:00 2025.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

The Enhanced Listbox documentation
==================================
Written by Greg Walters
-----------------------

.. image:: /images/EnhancedListboxDemo.png
    :scale: 50%
    


How it works
*************

There are basically 5 functions that you need to copy out of the support module and place into your own file. 

These are:

* do_enhlistbox_bindings
* load_listbox
* on_lbmotion
* manage_favorites

In your startup function, you need to make two calls.  The first is to setup the bindings and the second is the function that sets up the colors for the various parts of the listbox and the font that is used to display the items in the listbox.

.. code-block:: bash

    do_enhlistbox_bindings
    load_listbox(ListItems={yourlisthere})
    
We'll look at each of the functions one at a time and see any parameters you need to provide.


do_enhlistbox_bindings
+++++++++++++++++++++++

.. code-block:: python

 def do_enhlistbox_bindings():
    global listbox
    listbox = _w1.Scrolledlistbox1
    global favList
    favList = []
    listbox.bind("<Motion>", on_lbmotion)
    listbox.bind("<Button-3>", manage_favorites)





load_listbox
++++++++++++++



.. code-block:: python

 def load_listbox(
    ListboxBackground="lightgoldenrodyellow",
    SelectionBackground="seagreen2",
    FavoriteBackground="skyblue2",
    ListItems=[],):

    global listboxBg, selectBg, favBg, listitems
    listboxBg = ListboxBackground
    selectBg = SelectionBackground
    favBg = FavoriteBackground
    listbox.configure(background=listboxBg)
    listbox.configure(selectbackground=selectBg)
    listbox.configure(selectborderwidth=2)
    listbox.configure(activestyle=NONE)
    listbox.configure(font="-family {DejaVuSansMono} -size 10")
    listbox.configure(takefocus=0)
    for itm in ListItems:
        _w1.Scrolledlistbox1.insert("end", itm)




on_lbmotion
++++++++++++

.. code-block:: python

 def on_lbmotion(event):
    global listbox
    global mousex, mousey
    x, y = event.x, event.y
    mousex = x
    mousey = y



manage_favorites
+++++++++++++++++


.. code-block:: python

 def manage_favorites(*args):
    global textbox, listbox
    global favList
    global listboxBg, selectBg, favBg

    if _debug:
        print("Into function manage_favorites")
        print(args)
        for arg in args:
            print(f"   {arg}")
    global mousex, mousey
    X = listbox.winfo_pointerx()
    y = listbox.winfo_pointery()
    pos = listbox.nearest(mousey)
    indx = listbox.curselection()
    itm = listbox.get(pos)
    tagname = itm

    if itm in favList:
        favList.remove(itm)
        # listbox.itemconfig(pos, background="lightgoldenrodyellow")
        listbox.itemconfig(pos, background=listboxBg)
        # _w1.StatusInfo2.set(f"  User removed {itm} from list")
    else:
        favList.append(itm)
        # listbox.itemconfigure(pos, background="skyblue2")
        listbox.itemconfigure(pos, background=favBg)
        # _w1.StatusInfo2.set(f"  User added {itm} to list")





Add your content using ``reStructuredText`` syntax. See the
`reStructuredText <https://www.sphinx-doc.org/en/master/usage/restructuredtext/index.html>`_
documentation for details.




.. toctree::
   :maxdepth: 2
   :caption: Contents:

