### API: Documentation, authentication & versioning

Readup currently has an undocumented internal API, which is usable by users only via session cookies.

To let users integrate Readup with other services in a more convenient way, it would be good to address the following aspects:
- Documentation: documenting endpoints, inputs, and possible outputs of API endpoints.
- Authentication: permanent personal access tokens, configurable in Readup's UI (or via an API), are a convenient way to connect third-party services.
- Versioning: exposing a stable API that can be changed in major ways by creating new versions for it.

### Export 

For archiving or analyzing your own data, it would be great if Readup supported a "data takeout" feature, with which you can download a machine-readable version of your data in Readup (in `json` or `csv`).