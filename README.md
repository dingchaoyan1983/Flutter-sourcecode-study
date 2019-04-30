# Flutter-sourcecode-study
```
  /// Create an element for the given widget and add it as a child of this
  /// element in the given slot.
  ///
  /// This method is typically called by [updateChild] but can be called
  /// directly by subclasses that need finer-grained control over creating
  /// elements.
  ///
  /// If the given widget has a global key and an element already exists that
  /// has a widget with that global key, this function will reuse that element
  /// (potentially grafting it from another location in the tree or reactivating
  /// it from the list of inactive elements) rather than creating a new element.
  ///
  /// The element returned by this function will already have been mounted and
  /// will be in the "active" lifecycle state.
  Element inflateWidget(Widget newWidget, dynamic newSlot) {
    assert(newWidget != null);
    final Key key = newWidget.key;
    if (key is GlobalKey) {
      final Element newChild = _retakeInactiveElement(key, newWidget);
      if (newChild != null) {
        assert(newChild._parent == null);
        assert(() { _debugCheckForCycles(newChild); return true; }());
        newChild._activateWithParent(this, newSlot);
        final Element updatedChild = updateChild(newChild, newWidget, newSlot);
        assert(newChild == updatedChild);
        return updatedChild;
      }
    }
    final Element newChild = newWidget.createElement();
    assert(() { _debugCheckForCycles(newChild); return true; }());
    newChild.mount(this, newSlot);
    assert(newChild._debugLifecycleState == _ElementLifecycle.active);
    return newChild;
  }
```
