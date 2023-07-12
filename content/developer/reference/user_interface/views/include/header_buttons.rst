.. rst-class:: o-definition-list

``<button>``
  Defines custom buttons similar to :ref:`list view buttons <reference/user_interface/views/list/button>` in the control panel
  that perform an action/call a model's method. The buttons which accepts an extra attribute when placed in a `header`:

  :display:
    string_ chooses from ``display`` or ``always`` (default: ``display``)

    By default, those buttons are only displayed when some records are
    selected, and they apply on the selection. When the attribute ``display``
    is set to ``always``, the button is available all the time, even if there's
    no selection.

  .. code-block:: xml

    <header>
        <button name="toDoAlways" type="object" string="Always displayed" display="always"/>
        <button name="toDoSelection" type="object" string="Displayed if selection"/>
    </header>
