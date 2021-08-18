

1) Program to an interface, not an implementation
2) Reduce the surface area of your interfaces



# OO Design

Passthrough methods indicate a conflation of responsibilities.

If an entire implementation of an interface is passthrough, then the thing being passed to really is the thing passing. Can the interface handle this functionality via default implementations (perhaps with one method that you must implement returning something that the default implementations will use)?
