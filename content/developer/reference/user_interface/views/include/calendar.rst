.. _reference/user_interface/views/calendar:

Calendar
========

Calendar views display records as events in a daily, weekly, monthly or yearly
calendar.

.. note:: By default the calendar view will be centered around the current date
   (today). You can pass a specific initial date to the context of the action in
   order to set the initial focus of the calendar on the period (see `mode`) around
   this date (the context key to use being `initial_date`)

Their root element is ``<calendar>``. Available attributes_ on the
calendar view are:

:string:
  string_ (default: ``''``)

  This view title is displayed only if you open an action that has no name and
  whose target is 'new' (opening a dialog)

:create:
  boolean_ (default: ``True``)

  Disable/enable record creation on the view.

:edit:
  boolean_ (default: ``True``)

  Disable/enable record editing on the view.

:delete:
  boolean_ (default: ``True``)

  Disable/enable record deletion on the view through the **Action** dropdown.

.. rst-class:: o-definition-list

``date_start`` (required)
    name of the record's field holding the start date for the event
``date_stop``
    name of the record's field holding the end date for the event, if
    ``date_stop`` is provided records become movable (via drag and drop)
    directly in the calendar
``date_delay``
    alternative to ``date_stop``, provides the duration of the event instead of
    its end date (unit: day)
``color``
    name of a record field to use for *color segmentation*. Records in the
    same color segment are allocated the same highlight color in the calendar,
    colors are allocated semi-randomly.
    Displayed the display_name/avatar of the visible record in the sidebar
``form_view_id``
    view to open when the user create or edit an event. Note that if this attribute
    is not set, the calendar view will fall back to the id of the form view in the
    current action, if any.
``event_open_popup``
    If the option 'event_open_popup' is set to true, then the calendar view will
    open events (or records) in a FormViewDialog. Otherwise, it will open events
    in a new form view (with a do_action)
``quick_add``
    enables quick-event creation on click: only asks the user for a ``name``
    (the field to which this values is saved can be controlled through
    ``rec_name``) and tries to create a new event with just that and the clicked
    event time. Falls back to a full form dialog if the quick creation fails
``create_name_field``
    name of the record's field holding the textual representation of the record,
    this is used when creating records through the 'quick create' mechanism
``all_day``
    name of a boolean field on the record indicating whether the corresponding
    event is flagged as day-long (and duration is irrelevant)
``mode``
    Default display mode when loading the calendar.
    Possible attributes are: ``day``, ``week``, ``month``, ``year``
``scales``
    Comma-separated list of scales to provide. By default, all scales are
    available. See mode for possible scale values.
``create``, ``delete``
    allows disabling the corresponding action in the view by setting the
    corresponding attribute to ``false``
``<field>``
  declares fields to aggregate or to use in kanban *logic*. If the field is
  simply displayed in the calendar cards.

  Fields can have additional attributes:

  .. rst-class:: o-definition-list

  ``invisible``
    use "True" to hide the value in the cards
  ``avatar_field``
    only for x2many field, to display the avatar instead of the display_name
    in the cards
  ``write_model`` and ``write_field`` and ``filter_field``
    you can add a filter and save the result in the defined model, the
    filter is added in the sidebar. The ``filter_field`` is optional and allows
    you to specify the field that will hold the status of the filter.
  ``filters`` and ``color``
    use "True" to add this field in filter in the sidebar. You can specify
    a ``color`` field used to colorize the checkbox.


Model Commons
-------------

.. currentmodule:: odoo.addons.base.models.ir_ui_view
.. autoattribute:: Model._date_name
    :noindex:

.. _attributes: https://en.wikipedia.org/wiki/HTML_attribute
.. _boolean: https://docs.python.org/3/library/stdtypes.html#boolean-values
.. _string: https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str
