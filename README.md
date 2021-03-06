A flexible implementation of the observer pattern with automatic lifetime management.

Example usage:
```cpp
#include "observer.hpp"

#include <iostream>
#include <string>
#include <functional>

using namespace ::std;
using namespace ::std::placeholders;
using namespace ::obs;

struct HubbleTelescope
{
  void foundNewPlanet(const int type, const string name) const
  {
    cout << "The hubble telescope found a new planet of type " 
         << type << " named " << name << endl;
  }
};

int main()
{
  Subject<void (int, string)> space;

  // E.T. is watching space
  auto E_T_ = space.registerObserver( [] (int, string name) 
    {
      cout << "E.T. phone " << name << "?" << endl;
    });

  {
    // The hubble telescope is watching only a portion of space
    HubbleTelescope ht;
    auto registration = space.registerObserver(
      bind(&HubbleTelescope::foundNewPlanet, cref(ht), _1, _2));

    space(2, "Mogo"); 
  }

  space(3, "Oa");
}
```

More information can be found here: http://eviltwingames.com/blog/?p=20
