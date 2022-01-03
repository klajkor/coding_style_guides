Embedded C Coding Style Guide
=============================


About This Guide
----------------

* Purpose of this style guide is to encourage use of common coding practices for developing in embedded C.
* This document mainly based on `Espressif IoT Development Framework Style Guide <https://docs.espressif.com/projects/esp-idf/en/latest/esp32/contribute/style-guide.html>`__, but there are some differences.
* Style guide is a set of rules which are aimed to help create readable, maintainable, and robust code.
* By writing code which looks the same way across the code base we help others read and comprehend the code.
* By using same conventions for spaces and newlines we reduce chances that future changes will produce huge unreadable diffs.
* By following common patterns for module structure and by using language features consistently we help others understand code behavior.


C Code Formatting
-----------------

Naming
^^^^^^

* Any variable or function which is only used in a single source file should be declared ``static``.
* Public names (non-static variables and functions) should be namespaced with a per-component or per-unit prefix, to avoid naming collisions. ie ``core_get_node_id()`` or ``esp_console_run()``. Starting the prefix with ``esp_`` for Espressif-specific names is optional, but should be consistent with any other names in the same component.
* Avoid unnecessary abbreviations (ie shortening ``data`` to ``dat``), unless the resulting name would otherwise be very long.


Indentation
^^^^^^^^^^^

Use 4 spaces for each indentation level. Don't use tabs for indentation. Configure the editor to emit 4 spaces each time you press tab key.

Vertical Space
^^^^^^^^^^^^^^

Place one empty line between functions. Don't begin or end a function with an empty line.

.. code-block::

   void function()                 // correct
    {
        do_one_thing();
        do_another_thing();
    }

    void function1()
    {
        do_one_thing();
        do_another_thing();
                                    // INCORRECT, don't place empty line before the closing brace
    }
                                    // place empty line here
    void function2()
    {
                                    // INCORRECT, don't use an empty line after the opening brace
        int var = 0;
        while (var < SOME_CONSTANT) {
            do_stuff(&var);
        }
    }

The maximum line length is 120 characters as long as it doesn't seriously affect the readability.

Horizontal Space
^^^^^^^^^^^^^^^^

Always add single space after conditional and loop keywords

.. code-block::

    if (condition)      // correct
    {
        // ...
    }

    switch (n)          // correct
    {
        case 0:
            // ...
    }

    for(int i = 0; i < CONST; ++i)      // INCORRECT
    {
        // ...
    }

Add single space around binary operators. No space is necessary for unary operators. It is okay to drop space around multiply and divide operators

.. code-block::

    const int y = y0 + (x - x0) * (y1 - y0) / (x1 - x0);    // correct

    const int y = y0 + (x - x0)*(y1 - y0)/(x1 - x0);        // also okay

    int y_cur = -y;                                         // correct
    ++y_cur;

    const int y = y0+(x-x0)*(y1-y0)/(x1-x0);                // INCORRECT


No space is necessary around ``.`` and ``->`` operators.

Sometimes adding horizontal space within a line can help make code more readable. For example, you can add space to align function arguments

.. code-block::

    core__gpio_connect_in_signal(PIN_CAM_D6,   I2S0I_DATA_IN14_IDX, false);
    core__gpio_connect_in_signal(PIN_CAM_D7,   I2S0I_DATA_IN15_IDX, false);
    core__gpio_connect_in_signal(PIN_CAM_HREF, I2S0I_H_ENABLE_IDX,  false);
    core__gpio_connect_in_signal(PIN_CAM_PCLK, I2S0I_DATA_IN15_IDX, false);

- Note however that if someone goes to add new line with a longer identifier as first argument (e.g.  ``PIN_CAM_VSYNC``), it will not fit. So other lines would have to be realigned, adding meaningless changes to the commit.
- Therefore, use horizontal alignment sparingly, especially if you expect new lines to be added to the list later.
- **Never** use TAB characters for horizontal alignment.
- **Never** add trailing whitespace at the end of the line.

Braces
^^^^^^

- Function definition should have a brace on a separate line

.. code-block::

    // This is correct:
    void function(int arg)
    {

    }

    // NOT like this:
    void function(int arg) {

    }

- Within a function, place opening brace on a separate line with conditional and loop statements

.. code-block::

    if (condition)
    {
        do_one();
    }
    else if (other_condition)
    {
        do_two();
    }

Comments
^^^^^^^^

Use ``//`` for single line comments. For multi-line comments it is okay to use either ``//`` on each line or a ``/* */`` block.

Although not directly related to formatting, here are a few notes about using comments effectively.

- Don't use single comments to disable some functionality

::

    void init_something()
    {
        setup_dma();
        // load_resources();                // WHY is this thing commented, asks the reader?
        start_timer();
    }

- If some code is no longer required, remove it completely. If you need it you can always look it up in git history of this file. If you disable some call because of temporary reasons, with an intention to restore it in the future, add explanation on the adjacent line

::

    void init_something()
    {
        setup_dma();
        // TODO: we should load resources here, but loader is not fully integrated yet.
        // load_resources();
        start_timer();
    }

- Same goes for ``#if 0 ... #endif`` blocks. Remove code block completely if it is not used. Otherwise, add comment explaining why the block is disabled. Don't use ``#if 0 ... #endif`` or comments to store code snippets which you may need in the future.

- Don't add trivial comments about authorship and change date. You can always look up who modified any given line using git. E.g. this comment adds clutter to the code without adding any useful information

::

    void init_something()
    {
        setup_dma();
        // XXX add 2016-09-01
        init_dma_list();
        fill_dma_item(0);
        // end XXX add
        start_timer();
    }

Line Endings
^^^^^^^^^^^^

- Commits should only contain files with LF (Unix style) endings.
- Windows users can configure git to check out CRLF (Windows style) endings locally and commit LF endings by setting the ``core.autocrlf`` setting. `Github has a document about setting this option <github-line-endings>`.
- If you accidentally have some commits in your branch that add LF endings, you can convert them to Unix by running this command in an MSYS2 or Unix terminal (change directory to the IDF working directory and check the correct branch is currently checked out, beforehand):

.. code-block:: bash

  git rebase --exec 'git diff-tree --no-commit-id --name-only -r HEAD | xargs dos2unix && git commit -a --amend --no-edit --allow-empty' master

(Note that this line rebases on master, change the branch name at the end to rebase on another branch.)

For updating a single commit, it's possible to run ``dos2unix FILENAME`` and then run ``git commit --amend``
