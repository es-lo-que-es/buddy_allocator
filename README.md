<h3> buddy allocator </h3>

<p><i>buddy allocator i wrote in odin for my wasm projects</i></p>

<p> it is implemented using implicit free-list for the sake of simplicity </p>


```Odin
package buddy_allocator

MIN_POWER :: 6;
MAX_POWER :: 12;

MIN_BLOCK :: 1 << MIN_POWER;
MAX_BLOCK :: 1 << MAX_POWER;

DEPTH :: MAX_POWER - MIN_POWER + 1;

// NOTE: or u could call buddy_allocator.data_size(min, max)
// to get needed data size instead of calculating it urself
DATA_SIZE :: MAX_BLOCK + DEPTH * size_of(^Node);
MEMORY: [DATA_SIZE]u8;


main :: proc()
{
   buddy: BuddyAllocator;

   ok := init(&buddy, MEMORY[:], MIN_BLOCK, MAX_BLOCK);
   if !ok do panic("failed to init buddy allocator\n");

   context.allocator = buddy_allocator(&buddy);

   s := make([]u8, 20);
   print_debug_data(&buddy);
   delete(s);
}

```
