# Swift Style Guide

The purpose of this guide is, first-and-foremost, to help provide clarity when reading and writing code that can easily be understood by multiple developers working on a single project. The Swift programming language allows for a style unlike many other languages, however, this allowed style normally decreases the clarity of the code, especially to developers who aren’t as familiar with it. (Examples: Not wrapping control conditions in (), not ending statements with semi-colons (;)) This guide will help resolve these issues and lead to a more productive developer experience.

- Author: [Jaron Lowe](https://github.com/Jaron-Lowe)
- Version: 1.0

## General

### Statements:
1. All statements should end with a semi-colon (;). This provides clarity of where statements end.
2. A statement should never be place on the same line as another statement.

### Variables:
1. All variables should indicate their desired type. This helps prevent errors and leads to clarity for other developers.
2. All variables should be initialized clearly (ie. var myBool:Bool = false; vs. var myBool:Bool = Bool();).
3. All variables and types should be specified by their shorthand versions.
4. Variables should only be force unwrapped if already checked against nil. Otherwise, all variables should be optionally unwrapped.
5. All properties should be prefixed with self when used. This makes it clear that it is an instance-type variable being used.

**Preferred:**


```swift
var strings:[String] = [];

var jsonData:[String:AnyObject] = [];
var view:UIView? = nil;
jsonData = self.getDataAndPopulateView(view:UIView);
if (view != nil) {
    print("Data: \(jsonData)");
    view!.hidden = false;
    view!.customValue = self.myProperty;
}
```

**Not Preferred:**


```swift
var strings = [String]()
or
var strings:Array<String> = [String]()
```



### Control Flow:

#### General:
1. All blocks/closures/control structures should be contained in {}.
2. All block brackets should be contained on the same line as functions/methods & control statements (if/else/for/while/etc).

#### if:
1. if/else if conditions should be surrounded by (). This provides clarity between condition and statement.
2. if/else if statements must always be surrounded by {}.
3. If a single statement is executed inside an if/else if {}, then it can be contained on a single line, otherwise statements should be on their own lines.

#### for:
1. for-in conditions cannot be wrapped in ().
2. Classic for conditions should be wrapped in ().

**Preferred:**

```swift
if (myCondition == true) {return true;}
else if (yourCondition == true) {
    print("Your condition is true.");
    return true;
} else {return false;}
for element:[String:AnyObject] in self.elements {print("Element: \(element)");}
for (var j = 0; j < self.elements.count; j++) {print("Element: \(self.elements[j])");}
```

**Not Preferred:**

```swift
if myCondition == true 
    return true
else if yourCondition == true 
{
    print("Your condition is true.")
    return true
} 
else { return false; }
for element in self.elements 
{
    print("Element: \(element)")
}
for var j = 0; j < self.elements.count; j++
{
    print("Element: \(self.elements[j])")
}
```



## Class Layout

### Property Declaration:
1. Property values should be separated into at least 2 subgroups: IBOutlets and Variables

**Preferred:**

```swift
//=================================================================================
// MARK: - Properties
//=================================================================================

// IBOutlets
@IBOutlet weak var tableView: UITableView!;
@IBOutlet weak var searchBar: UISearchBar!;
@IBOutlet weak var keyboardHideButton: UIButton!;

// Variables
var sourceLeads:[SMLead] = [];
var droppedIndex:Int? = nil;
var sortAsc:Bool = true;
var sortType:LeadSortType = LeadSortType.FirstName;
```

**Not Preferred:**

```swift

@IBOutlet weak var tableView: UITableView!;
var sourceLeads:[SMLead] = [];
var droppedIndex:Int? = nil;

@IBOutlet weak var searchBar: UISearchBar!;
var sortAsc:Bool = true;
var sortType:LeadSortType = LeadSortType.FirstName;

@IBOutlet weak var keyboardHideButton: UIButton!;
```

### Methods:
1. Similar methods should be grouped together.
2. Groups should be preface with a MARK comment:
3. Methods should be separated with one line break in between.
4. Two line breaks can be used between methods to indicate a subgroup of methods.
5. Each group should be ended with two line breaks.
6. IBActions should clearly express the event used to trigger them (ie. myButtonPressed(), myButtonDragged()).
7. All methods should be prefixed with self when used. This helps to prevent a failure to do so when handling asynchronous code.

**Preferred:**

```swift
// =================================================================================
// MARK: - IBActions
// =================================================================================

@IBAction func addNewLead(sender: UIBarButtonItem) {...}

@IBAction func sortButtonPressed(sender: AnyObject) {...}

@IBAction func sortSegmentPressed(sender: UISegmentedControl) {...}


// Added Programmatically
@IBAction func dropDownButtonPressed(sender:UIButton) {...}

@IBAction func dropDownButtonDragged(sender:UIButton) {...}

@IBAction func refreshTable(sender:UIRefreshControl) {...}


// =================================================================================
// MARK: - Action Methods
// =================================================================================

// API Actions
func getLeads() {...}

func changeLeadStatus(index:Int?, status:LeadFilterType) {...}


// Normal Actions
func showEditLead(lead:SMLead) {...}

func showLeadInfo(lead:SMLead) {...}


// Sort/Filter Actions
func sortLeadsByType(type:LeadSortType) {...}

func filterLeadsByType(type: LeadFilterType) {...}


```

**Not Preferred:**

```swift

@IBAction func addNewLead(sender: UIBarButtonItem) {...}

@IBAction func sortButton(sender: AnyObject) {...}
@IBAction func sortSegment(sender: UISegmentedControl) {...}

func getLeads() {...}
func changeLeadStatus(index:Int?, status:LeadFilterType) {...}

@IBAction func refreshTable(sender:UIRefreshControl) {...}
func showEditLead(lead:SMLead) {...}
func showLeadInfo(lead:SMLead) {...}

func sortLeadsByType(type:LeadSortType) {...}
func filterLeadsByType(type: LeadFilterType) {...}

@IBAction func dropDownButton(sender:UIButton) {...}
@IBAction func dropDownButtonDragged(sender:UIButton) {...}

```

### Protocol/Interface/Delegate Handling:
1. Classes using Protocol Interface Delegation should pair delegate methods into a separate class extension.

**Preferred:**

```swift
class MyLeadsViewController: JViewController {...}

// =================================================================================
// MARK: - UISearchBarDelegate
// =================================================================================

extension MyLeadsViewController: UISearchBarDelegate {
    
    func searchBar(searchBar: UISearchBar, textDidChange searchText: String) {...}
    
    func searchBarTextDidEndEditing(searchBar: UISearchBar) {...}
    
    func searchBarSearchButtonClicked(searchBar: UISearchBar) {...}
    
    
}


// =================================================================================
// MARK: - UIActionSheetDelegate
// =================================================================================

extension MyLeadsViewController: UIActionSheetDelegate {
    
    func actionSheet(actionSheet: UIActionSheet, clickedButtonAtIndex buttonIndex: Int) {...}
    
    
}
```

**Not Preferred:**

```swift

class MyLeadsViewController: JViewController, UISearchBarDelegate, UIActionSheetDelegate {
    
    ...
    
    // =================================================================================
    // MARK: - UISearchBarDelegate
    // =================================================================================
        
    func searchBar(searchBar: UISearchBar, textDidChange searchText: String) {...}
    
    func searchBarTextDidEndEditing(searchBar: UISearchBar) {...}
    
    func searchBarSearchButtonClicked(searchBar: UISearchBar) {...}
    
    
    // =================================================================================
    // MARK: - UIActionSheetDelegate
    // =================================================================================
        
    func actionSheet(actionSheet: UIActionSheet, clickedButtonAtIndex buttonIndex: Int) {...}
    
    ...
    
}
```


### Standard Class Layout:

- Imports
- Protocol Declarations
- Enum Declations
- Class Declartion
  - Properties
    - IBOutlets
    - Variables
  - Class Initialization
  - Superclass Overwriting Methods
  - IBActions
  - Action Methods - Contains the logic of a feature or action.
  - Helper Methods - Contains methods used to supplement the Action Methods.
- Class Extensions



## Code Labeling & Markup

### Pragma Marks & Swift MARK:s
Code files should be divided into relevant sections using pragma marks and in Swift MARK:s

### Debug Code
Debug code that can be used at a later date may be commented out and marked with "* DEBUG *".

### Deprecations
Old/unused code that may have some important purpose may be commented "* DEPRECATED *" to be revisted/deleted at a later date.

### To-Do
Areas where code is needed to finish a feature or some functionality need to be marked with "* TODO *" to be revisited at the soonest opportunity. It is helpful to comment what needs to be done in the general area as well.

### Localizations
Strings that have not been localized need to be marked with "* LOCALIZE *"

### Stats
Points in the app where stats are to be - and/or are being tracked should be marked with "* STAT *".

### Features
Areas where features are turned on/off by the mobile config in code should be marked with "* FEATURE *".
