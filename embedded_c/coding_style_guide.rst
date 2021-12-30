Embedded C Coding Style Guide
=============================


About This Guide
----------------

Purpose of this style guide is to encourage use of common coding practices for developing in embedded C.
This document mainly based on `Espressif IoT Development Framework Style Guide <https://docs.espressif.com/projects/esp-idf/en/latest/esp32/contribute/style-guide.html>`__

Style guide is a set of rules which are aimed to help create readable, maintainable, and robust code.
By writing code which looks the same way across the code base we help others read and comprehend the code.
By using same conventions for spaces and newlines we reduce chances that future changes will produce huge unreadable diffs.
By following common patterns for module structure and by using language features consistently we help others understand code behavior.


C Code Formatting
-----------------

Naming
^^^^^^

* Any variable or function which is only used in a single source file should be declared ``static``.
* Public names (non-static variables and functions) should be namespaced with a per-component or per-unit prefix, to avoid naming collisions. ie ``esp_vfs_register()`` or ``esp_console_run()``. Starting the prefix with ``esp_`` for Espressif-specific names is optional, but should be consistent with any other names in the same component.
* Static variables should be prefixed with ``s_`` for easy identification. For example, ``static bool s_invert``.
* Avoid unnecessary abbreviations (ie shortening ``data`` to ``dat``), unless the resulting name would otherwise be very long.


Indentation
^^^^^^^^^^^

Use 4 spaces for each indentation level. Don't use tabs for indentation. Configure the editor to emit 4 spaces each time you press tab key.

Vertical Space
^^^^^^^^^^^^^^

Place one empty line between functions. Don't begin or end a function with an empty line.

.. code-block::

   void function()                 // CORRECT
    {
        do_one_thing();
        do_another_thing();
    }

    void function1()
    {
        do_one_thing();
        do_another_thing();
                                    // INCORRECT, don't place empty line here
    }
                                    // place empty line here
    void function2()
    {
                                    // INCORRECT, don't use an empty line here
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


