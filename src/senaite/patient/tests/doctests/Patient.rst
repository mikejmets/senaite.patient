Patient Object
--------------

Running this test from the buildout directory:

    bin/test test_textual_doctests -t Patient


Test Setup
..........

Needed Imports:

    >>> from plone import api as plone_api
    >>> from bika.lims import api
    >>> from senaite.patient import api as patient_api

Variables:

    >>> portal = self.portal
    >>> request = self.request
    >>> setup = api.get_setup()
    >>> patients = portal.patients


Patients
--------

All patients are located in the patients folder:

    >>> patient = api.create(patients, "Patient", mrn="1")
    >>> patient
    <Patient at /plone/patients/P000001>

A patient can have a firstname, middlename and lastname:

    >>> api.edit(patient, firstname="Bruce", middlename="Anthony", lastname="Wayne")
    >>> patient.getFullname()
    'Bruce Anthony Wayne'

A patient can have a primary and additional email addresses:

    >>> api.edit(patient, email="bruce@example.com",
    ...          additional_emails=[{"name": "Work", "email": "wayne@example.com"}])

    >>> patient.getEmail()
    'bruce@example.com'

    >>> patient.getAdditionalEmails()
    [{'name': 'Work', 'email': 'wayne@example.com'}]

Create patient with no MRN should fail:
    >>> patient_api.is_patient_required()
    True
    >>> patient = api.create(patients, "Patient")
    >>> patient
    None
    >>> plone_api.portal.set_registry_record("senaite.patient.require_patient", False)
    >>> patient_api.is_patient_required()
    False
    >>> patient = api.create(patients, "Patient")
    >>> patient
    <Patient at /plone/patients/P000002>

Create patient with Patient ID:
    >>> patient = api.create(patients, "Patient", patient_id='1234')
    >>> patient
    <Patient at /plone/patients/P000003>
    >>> patient.getPatientID()
    '1234'
