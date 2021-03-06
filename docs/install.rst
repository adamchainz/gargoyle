Installation
============

Install using pip:

.. code-block:: sh

    pip install gargoyle-yplan

If you are upgrading from the original to this fork, you will need to run the following first, since the packages
clash:

.. code-block:: bash

    pip uninstall django-modeldict gargoyle

Failing to do this will mean that ``pip uninstall gargoyle`` will also erase the files for `gargoyle-yplan`, and
similarly for our `django-modeldict` fork.

Enable Gargoyle
---------------

Once you've downloaded the Gargoyle package, you simply need to add it to your ``INSTALLED_APPS``:

.. code-block:: python

    INSTALLED_APPS = (
        ...
        'gargoyle',
    )

Gargoyle has autodiscovery similar to Django Admin - it will look in each of your ``INSTALLED_APPS`` for a
``gargoyle`` submodule, and import that. You can use this to declare extra ``ConditionSet``\s. If you use such
submodules and Python 2.7, you'll need to ensure your imports are not relative in those files:

.. code-block:: python

    # myapp.gargoyle
    from __future__ import absolute_import

    from gargoyle.conditions import ConditionSet

Nexus Frontend
--------------

While Gargoyle can be used without a frontend, we highly recommend using `Nexus <https://github.com/adamchainz/nexus>`_.

Nexus will automatically detect Gargoyle's ``NexusModule``, assuming its autodiscovery is on. If not, you will need to
register the module by hand:

.. code-block:: python

    from gargoyle.nexus_modules import GargoyleModule

    nexus.site.register(GargoyleModule, 'gargoyle')

Disabling Auto Creation
-----------------------

Under some conditions you may not want Gargoyle to automatically create switches that don't currently exist. To disable
this behavior, you may use the ``GARGOYLE_AUTO_CREATE`` setting your ``settings.py``:

.. code-block:: python

    GARGOYLE_AUTO_CREATE = False

Default Switch States
~~~~~~~~~~~~~~~~~~~~~

The ``GARGOYLE_SWITCH_DEFAULTS`` setting allows engineers to set the default state of a switch before it's been added
via the gargoyle admin interface. Set the ``initial_status`` value to set it to a specific switch status - one of
``DISABLED``, ``SELECTIVE``, ``GLOBAL``, or ``INHERIT``. You can also use the legacy key ``is_active``, with value
``True`` to make it ``GLOBAL``, and ``False`` to make it ``DISABLED``. For example:

.. code-block:: python
    from gargoyle.constants import DISABLED, SELECTIVE

    GARGOYLE_SWITCH_DEFAULTS = {
        'new_switch': {
          'initial_status': DISABLED,
          'label': 'Funky Switch',
          'description': 'Controls the funkiness.',
        },
        'another_switch': {
          'initial_status': SELECTIVE,
          'label': 'Conditional Funky Switch',
          'description': 'Controls more funkiness.',
        },
        'deprecated_format_new_switch': {
          'is_active': True,
          'label': 'Old funkiness',
          'description': 'Defaults on but should really use initial_status=GLOBAL',
        },
    }

