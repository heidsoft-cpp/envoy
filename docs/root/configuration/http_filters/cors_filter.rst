.. _config_http_filters_cors:

CORS
====

This is a filter which handles Cross-Origin Resource Sharing requests based on route or virtual host settings.
For the meaning of the headers please refer to the pages below.

* https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS
* https://www.w3.org/TR/cors/
* :ref:`v2 API reference <envoy_api_msg_route.CorsPolicy>`
* This filter should be configured with the name *envoy.cors*.

.. _cors-runtime:

Runtime
-------

The CORS filter supports the following RuntimeFractionalPercent settings:

filter_enabled
  The % of requests for which the filter is enabled. The default is
  100/:ref:`HUNDRED <envoy_api_enum_type.FractionalPercent.DenominatorType>`.

  To utilize runtime to enabled/disable the CORS filter set the
  :ref:`runtime_key <envoy_api_msg_core.runtimefractionalpercent>`
  value of the :ref:`filter_enabled <envoy_api_field_route.CorsPolicy.filter_enabled>`
  field.

  .. note::

    If present, this will override the :ref:`enabled <envoy_api_field_route.CorsPolicy.enabled>`
    field of the configuration.

shadow_enabled
  The % of requests for which the filter is enabled in shadow only mode. Default is 0.
  If present, this will evaluate a request's *Origin* to determine if it's valid
  but will not enforce any policies.

  To utilize runtime to enabled/disable the CORS filter's shadow mode set the
  :ref:`runtime_key <envoy_api_msg_core.runtimefractionalpercent>`
  value of the :ref:`shadow_enabled <envoy_api_field_route.CorsPolicy.shadow_enabled>`
  field.

To determine if the filter and/or shadow mode are enabled you can check the runtime
values via the admin panel at :http:get:`/runtime`.

.. note::

  If both ``filter_enabled`` and ``shadow_enabled`` are on, the ``filter_enabled``
  flag will take precedence.

.. _cors-statistics:

Statistics
----------

The CORS filter outputs statistics in the <stat_prefix>.cors.* namespace.

.. note::
  Requests that do not have an Origin header will be omitted from statistics.

.. csv-table::
  :header: Name, Type, Description
  :widths: 1, 1, 2

  origin_valid, Counter, Number of requests that have a valid Origin header.
  origin_invalid, Counter, Number of requests that have an invalid Origin header.
