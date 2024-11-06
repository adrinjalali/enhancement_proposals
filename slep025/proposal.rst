.. _slep_025:

====================================
SLEP025: BaseEstimator.freeze method
====================================

:Author: Adrin Jalali
:Status: Draft
:Type: Standard
:Created: 2024-10-06

Abstract
--------
This SLEP proposes adding a ``freeze`` method to the ``BaseEstimator`` class.
[2]_ added a `sklearn.frozen.FrozenEstimator` class to scikit-learn. [3]_ started
a discussion on the implications of adding a ``FrozenEstimator`` to scikit-learn.

As a part of that discussion, it became clear that we'd like to use `FrozenEstimator`
instead of having patterns such as `cv="prefit"` or `prefit=True` in meta-estimators.
[4]_ and [5]_ are examples of how this change would impact the API.

However, this means user's code would look like the following::

    from sklearn.calibration import CalibratedClassifierCV
    from sklearn.ensemble import RandomForestClassifier
    from sklearn.frozen import FrozenEstimator

    clf = RandomForestClassifier(n_estimators=25)
    clf.fit(X_train, y_train)
    cal_clf = CalibratedClassifierCV(FrozenEstimator(clf), method="sigmoid")
    cal_clf.fit(X_valid, y_valid)

This SLEP proposes adding a ``freeze`` method to the ``BaseEstimator`` class, which
changes the above code to the following::

    from sklearn.calibration import CalibratedClassifierCV
    from sklearn.ensemble import RandomForestClassifier

    clf = RandomForestClassifier(n_estimators=25)
    clf.fit(X_train, y_train)
    cal_clf = CalibratedClassifierCV(clf.freeze(), method="sigmoid")
    cal_clf.fit(X_valid, y_valid)

The proposal of this SLEP is implemented in [6]_.

This change has no backward compatibility concerns.

Copyright
---------

This document has been placed in the public domain. [1]_

References and Footnotes
------------------------
.. [1] Open Publication License: https://www.opencontent.org/openpub/

.. [2] `Adding FrozenEstimator in #29705
   <https://github.com/scikit-learn/scikit-learn/pull/29705>`__

.. [3] `Implications of adding a FrozenEstimator'
   <https://github.com/scikit-learn/scikit-learn/issues/29893>`__

.. [4] `API deprecate CalibratedClassifierCV(..., cv=prefit) for FrozenEstimator #30171
   <https://github.com/scikit-learn/scikit-learn/pull/30171>`__

.. [5] `REVERT ENH add the parameter prefit in the FixedThresholdClassifier (#29067)
   #30172 <https://github.com/scikit-learn/scikit-learn/pull/30172>`

.. [6] ` ENH implement BaseEstimator.freeze #30211
   <https://github.com/scikit-learn/scikit-learn/pull/30211>`__
