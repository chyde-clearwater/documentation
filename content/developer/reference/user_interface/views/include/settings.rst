.. _reference/user_interface/views/settings:

Settings Form View
==================

The settings form view is a customization of the :ref:`form view <reference/user_interface/views/form>`.
It's used to centralize all the settings of Odoo.

This view differs from a generic form view because it has a search bar, a sidebar and accepts 3
additional tags: ``app``, ``block`` and ``setting``.

.. example::

  .. code-block:: xml

      <app string="CRM" name="crm">
          <setting type="header" string="Foo">
              <field name="foo" title="Foo?."/>
              <button name="nameAction" type="object" string="Button"/>
          </setting>
          <block title="Title of group Bar">
              <setting help="this is bar" documentation="/applications/technical/web/settings/this_is_a_test.html">
                  <field name="bar"/>
              </setting>
              <setting string="This is Big BAR" company_specific="1">
                  <field name="bar"/>
              </setting>
          </block>
          <block title="Title of group Foo">
              <setting string="Personalize setting" help="this is full personalize setting">
                  <div>This is a different setting</div>
              </setting>
          </block>
      </app>

.. _reference/user_interface/views/settings/app:

<app>: declare the application
------------------------------

.. code-block:: xml

  <form>
    <app string="NAME" name="TECHNICAL_NAME">
    ...
    </app>
  </form>

The ``app`` tag is used to declare the application on the settings view. It
creates an entry with its logo on the sidebar of the view. It also acts as
delimiter when searching. ``<app>`` can have the following attributes:

:string:
  string_ (mandatory)

  The name of the application.

:name:
  string_ (mandatory)

  The technical name of the application (the name of the module).

:logo:
  path_ (optional)

  The `relative path`_ to the logo. If not set, the logo is created using
  the ``name`` parameter : ``/{name}/static/description/icon.png``.

:groups:
  `Comma-separated values`_ (optional) whose choices are the :class:`~odoo.addons.base.models.res_users.Groups` reference

  same as for :ref:`field <reference/user_interface/views/form/field>` component.

:invisible:
  :ref:`python expression <user_interface/views/python_expression>` that evaluates to a boolean_ (default: ``False``)

  same as for :ref:`field <reference/user_interface/views/form/field>` component.

.. _reference/user_interface/views/settings/block:

<block>: declare a group of settings
------------------------------------

.. code-block:: xml

  <form>
    <app string="NAME" name="TECHNICAL_NAME">
      ...
      <block title="TITLE">
        ...
      </block>
      ...
    </app>
  </form>

The ``block`` tag is used to declare a group of settings. This group can have
a title and a description/help. ``<block>`` can have the following attributes:

:title:
  string_ (optional)

  The title of the block of settings, you can perform research on its text.

:help:
  string_ (optional)

  The description/help of the block of settings, you can perform research on
  its text.

:groups:
  `Comma-separated values`_ (optional) whose choices are the :class:`~odoo.addons.base.models.res_users.Groups` reference

  same as for :ref:`field <reference/user_interface/views/form/field>` component.

:invisible:
  :ref:`python expression <user_interface/views/python_expression>` that evaluates to a boolean_ (default: ``False``)

  same as for :ref:`field <reference/user_interface/views/form/field>` component.

.. _reference/user_interface/views/settings/setting:

<setting>: declare the setting
------------------------------

.. code-block:: xml

  <form>
    <app string="NAME" name="TECHNICAL_NAME">
      <block title="TITLE">
        ...
        <setting string="SETTING_NAME">
          ...
          <field name="FIELD_NAME"/>
          ...
        </setting>
        ...
      </block>
    </app>
  </form>

The ``setting`` tag is used to declare the setting itself. The first field in
the setting is used as the main field (optional). This field is placed on the
left panel (if it's a boolean field) or on the top of the right panel
(otherwise). The field is also used to create the setting label if a
``string`` is not defined. The ``setting`` tag can also contain more elements
(e.g. html), all of these elements are rendered in the right panel.
``<setting>`` can have the following attributes:

:type:
  string_ (optional)

  By default, a setting is visually separated on two panels (left and right),
  and is used to edit a given field. By defining ``type='header'``, a special
  kind of setting is rendered instead. This setting is used to modify the
  scope of the other settings. For example, on the website application, this
  setting is used to indicate to which website the other settings apply. The
  header setting is visually represented as a yellow banner on the top of the
  screen.

:string:
  string_ (optional)

  The text used as label of the setting. If it's not defined, the first field
  is used as label.

:title:
  string_ (optional)

  The text used as tooltip.

:help:
  string_ (optional)

  The help/description of the setting. This text is displayed just below the
  setting label (with classname ``text-muted``).

:company_dependent:
  ``1`` (optional)

  If this attribute is set to "1" an icon is displayed next to the setting
  label to explicit that this setting is company-specific.

:documentation:
  path_ (optional)

  If this attribute is set, an icon is added next to the setting label, this
  icon is a link to the documentation. Note that you can use relative or
  absolute path. The `relative path`_ is relative to ``https://www.odoo.com/documentation/<server_version>``,
  so it's not necessary to hard-code the server version on the arch anymore.

:groups:
  `Comma-separated values`_ (optional) whose choices are the :class:`~odoo.addons.base.models.res_users.Groups` reference

  same as for :ref:`field <reference/user_interface/views/form/field>` component.

:invisible:
  :ref:`python expression <user_interface/views/python_expression>` that evaluates to a boolean_ (default: ``False``)

  same as for :ref:`field <reference/user_interface/views/form/field>` component.


.. _`relative path`: https://en.wikipedia.org/wiki/URL
.. _path: https://en.wikipedia.org/wiki/Path_(computing)
.. _string: https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str
