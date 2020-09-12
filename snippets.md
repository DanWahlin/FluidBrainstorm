### #1 Extend DataObject

* Container for distributed data structures (DDSes).

### #2 Define Properties

* See https://fluidframework.com/docs/concepts/dds/#picking-the-right-data-structure for additional DDSes.
* DDSes/runtime ensure proper order as users contribute notes, votes, etc. Changes across clients are automatically handled.

```typescript
private notesMap: SharedMap;
private votesMap: SharedMap;
private usersMap: SharedMap;
```

### #3 Create Helper Function

* Used to create SharedMap objects

```typescript
private createSharedMap(id: string): void {
    const map = SharedMap.create(this.runtime);
    this.root.set(id, map.handle);
}
```

### #4 Create SharedMap Objects

```typescript
this.createSharedMap("notes");
this.createSharedMap("votes");
this.createSharedMap("users");
```

### #5 Assign Local References of SharedMap Objects

* Runs everytime client joins. Attach local props to created SharedMaps

```typescript
this.notesMap = await this.root.get<IFluidHandle<SharedMap>>("notes").get();
this.votesMap = await this.root.get<IFluidHandle<SharedMap>>("votes").get();
this.usersMap = await this.root.get<IFluidHandle<SharedMap>>("users").get();
```

### #6 Create Event Listeners Helper

```typescript
private createEventListeners(sharedMap: SharedMap): void {
    // Set up an event listener for changes to values in the SharedMap
    sharedMap.on("valueChanged", () => {
        this.emit("change");
    });
}
```