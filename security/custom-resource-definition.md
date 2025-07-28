## Custom Resource Definition
* CRDs are used to create custom resources / kubernetes objects.
```yaml
#Custom Resource - Example
apiVersion: flights.com/v1
kind: FlightTicket
metadata:
  name: flight-ticket
spec:
  from: Mumbai
  to: Chennai
  number: 2
```
```yaml
# custom resource definition - example
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: internals.datasets.kodekloud.com
spec:
  group: datasets.kodekloud.com
  versions:
    - name: v1
      served: true
      storage: true
      schema:
        openAPIV3Schema:
          type: object
          properties:
            spec:
              type: object
              properties:
                internalLoad:
                  type: string
                range:
                  type: integer
                percentage:
                  type: string
  scope: Namespaced
  names:
    plural: internal
    singular: internal
    kind: Internal
    shortNames:
    - int

```