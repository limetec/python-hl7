python-hl7 - Easy HL7 v2.x Parsing
==================================

python-hl7 is a simple library for parsing messages of Health Level 7 
(HL7) version 2.x into Python objects.

HL7 is a communication protocol and message format for 
health care data. It is the de-facto standard for transmitting data
between clinical information systems and between clinical devices.
The version 2.x series, which is often is a pipe delimited format
is currently the most widely accepted version of HL7 (version 3.0
is an XML-based format).

python-hl7 currently only parses HL7 version 2.x messages into
an easy to access data structure. The current implementation
does not completely follow the HL7 specification, but is good enough
to parse the most commonly seen HL7 messages. The library could 
potentially evolve into being fully complainant with the spec.
The library could eventually also contain the ability to create
HL7 v2.x messages.

python-hl7 parses HL7 into a series of wrapped :py:class:`hl7.Container` objects.
The there are specific subclasses of :py:class:`hl7.Container` depending on
the part of the HL7 message. The :py:class:`hl7.Container` message itself
is a subclass of a Python list, thus we can easily access the
HL7 message as an n-dimensional list. Specifically, the subclasses of
:py:class:`hl7.Container`, in order, are :py:class:`hl7.Message`, 
:py:class:`hl7.Segment`, and :py:class:`hl7.Field`.
Eventually additional containers will be added to fully support
the HL7 specification.

Usage
-----

As an example, let's create a HL7 message::

    >>> message = 'MSH|^~\&|GHH LAB|ELAB-3|GHH OE|BLDG4|200202150930||ORU^R01|CNTRL-3456|P|2.4\r'
    >>> message += 'PID|||555-44-4444||EVERYWOMAN^EVE^E^^^^L|JONES|196203520|F|||153 FERNWOOD DR.^^STATESVILLE^OH^35292||(206)3345232|(206)752-121||||AC555444444||67-A4335^OH^20030520\r'
    >>> message += 'OBR|1|845439^GHH OE|1045813^GHH LAB|1554-5^GLUCOSE|||200202150730||||||||555-55-5555^PRIMARY^PATRICIA P^^^^MD^^LEVEL SEVEN HEALTHCARE, INC.|||||||||F||||||444-44-4444^HIPPOCRATES^HOWARD H^^^^MD\r'
    >>> message += 'OBX|1|SN|1554-5^GLUCOSE^POST 12H CFST:MCNC:PT:SER/PLAS:QN||^182|mg/dl|70_105|H|||F\r'

We call the `hl7.parse()` command with string message::

    >>> h = parse(message)

We get a hl7.Message object, wrapping a series of hl7.Segment
objects::

    >>> type(h)
    <class 'hl7.Message'>

We can always get the HL7 message back::

    >>> str(h) == message.strip()
    True

Interestingly, this hl7.Message can be accessed as a list::

    >>> isinstance(h, list)
    True

There were 4 segments (MSH, PID, OBR, OBX):

    >>> len(h)
    4

We can extract the hl7.Segment from the hl7.Message instance::

    >>> h[3]
    [['OBX'], ['1'], ['SN'], ['1554-5', 'GLUCOSE', 'POST 12H CFST:MCNC:PT:SER/PLAS:QN'], [''], ['', '182'], ['mg/dl'], ['70_105'], ['H'], [''], [''], ['F']]

We can easily reconstitute this segment as HL7, using the
appopriate separators::

    >>> str(h[3])
    'OBX|1|SN|1554-5^GLUCOSE^POST 12H CFST:MCNC:PT:SER/PLAS:QN||^182|mg/dl|70_105|H|||F'

We can extract individual elements of the message::

    >>> h[3][3][1]
    'GLUCOSE'
    >>> h[3][5][1]
    '182'

We can look up segments by the segment identifier::

    >>> pid = segment('PID', h)
    >>> pid[3][0]
    '555-44-4444'


Contents
--------

.. toctree::
   :maxdepth: 1

   api
   contribute
   changelog
   license

Links
-----

* Documentation: http://python-hl7.readthedocs.org
* Source Code: http://github.com/johnpaulett/python-hl7
* PyPi: http://pypi.python.org/pypi/hl7

HL7 References:

* `Health Level 7 - Wikipedia <http://en.wikipedia.org/wiki/HL7>`_
* `nule.org's Introduction to HL7 <http://nule.org/wp/?page_id=99>`_
* `hl7.org <http://www.hl7.org/>`_
* `OpenMRS's HL7 documentation <http://openmrs.org/wiki/HL7>`_