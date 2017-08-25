# Xcode iOS Project Grouping Structure
This document outlines the grouping structure I use for my for iOS projects in Xcode.

This should not be seen as a definitive guide on keeping your Xcode project but as a window into my thinking and as an alternative to keep things tidy as it will **definitely** benefit you and your fellow developers. Your structure will and should change accordingly to the paradigm(MVC, MVVM, VIPER, etc.) you're using in your own project and your way of thinking/working.

Moral of the story: just try to keep things tidy.

## Xcode Grouping & Finder Folder Organisation

Prior to Xcode 9, Xcode didn't posses the ability to reflect the internal grouping structure to the foldering structure in Finder. Therefore I relied on a quite nice script written by [venmo](https://github.com/venmo) named [synx](https://github.com/venmo/synx) which in it's own words:
> A command-line tool that reorganizes your Xcode project folder to match your Xcode groups

This is a mission-critical thing since a cluttered structure may and probably will result in merge conflicts.

Now that Xcode 9 houses this functionality internally, it's safe to go without [synx](https://github.com/venmo/synx) and thank [venmo](https://github.com/venmo) for his much appreciated efforts. :]

## The Structure

- Project Name
	- **Controller:** For each controller I've, I create a separate group. The view controller file merely contains the bare minimum required for the VC to function. The rest is contained in extensions where those extensions conform to the related protocol. This greatly aids in the process of avoiding Massive-View-Controller symptoms. e.g.:
		- Map
			- MapVC.swift
			- MapViewDelegate.swift (e.g. `extension MapVC: MGLMapViewDelegate {}`)
			- MapViewTransitioningDelegate.swift (e.g. `extension MapVC: UIViewControllerTransitioningDelegate {}`)
		- Base Card Listing
			- BaseCardListingVC.swift
			- CardCell.swift (e.g. `final class CardCell: UICollectionViewCell {}`)
			- CardListingDataSource.swift (e.g. `extension BaseCardListingVC: UICollectionViewDataSource {}`)
			- CardListingDelegate.swift (e.g. `extension BaseCardListingVC: UICollectionViewDelegate {}`)
			- CardListingDelegateFlowLayout.swift (e.g. `extension BaseCardListingVC: UICollectionViewDelegateFlowLayout {}`)
	- **Dependency:** I tend to keep my dependencies to a minimum and therefore if it's a project where I'm not using any dependency managers(e.g. CocoaPods, Carthage,...), I keep my manually added dependencies here; each under it's own group name.
		- 1Password
		- SwiftKeychainWrapper
		- Mapbox
		- ...
	- **Extension:** Each extended entity MUST be in it's own group and under that grouping each file MUST contain a single extension.
		- Entity Name(e.g. `String`, `Double`, `UISearchBar`)
			- Entity Name + Extended Function Name (e.g. `UISearchBar+SetSearchIconColor.swift`, `Double+InMinutesSeconds.swift`)
	- **Model:** Basically, each model MUST have it's own group if it contains more than one file. If a model only consists of a single file, you can directly put it under the `Model` group.
		- Cache
		- Cached Web Service
		- Form Validation
		- Web Service
		- CoreDataManager.swift
	- **Other:** I tend to keep various miscellaneous stuff here like the `AppDelegate`, `Assets` folder, `Info.plist`, fonts, bundles and etc. Things that you won't visit quite often.
	- **Protocol**
		- Protocol Name (e.g. `Pop Up Presentable`)
			- Protocol Definition (e.g. `PopUpPresentable`)
			- Protocol Extension(s) (e.g. `PresentPopUp.swift`): Use method names defined in the protocol as file names.
	- **Store:** Usually the model layer is also used to store data but I experienced that concealing constants defined in enums and such in it's own grouping is a worthwhile endeavour.
	- **View:** Storyboards and programmatically generated views. If your files fall into it's own subcategories in regard to their naming conventions according to your project, do subgroup them as well in your own way.