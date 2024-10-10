import UIKit
import SideMenu

class ViewController: UIViewController, SideMenuNavigationControllerDelegate {

  override func viewDidLoad() {
    super.viewDidLoad()

    // Set up the SideMenu
    setupSideMenu()

    // Add a button to trigger the SideMenu
    let menuButton = UIBarButtonItem(title: "Menu", style: .plain, target: self, action: #selector(toggleSideMenu))
    self.navigationItem.leftBarButtonItem = menuButton
  }

  @objc func toggleSideMenu() {
    // Toggle the SideMenu
    present(SideMenuManager.default.menuLeftNavigationController!, animated: true)
  }

  func setupSideMenu() {
    // Create a new SideMenuNavigationController
    let menuViewController = MenuViewController() // Your custom menu view controller
    let menuNavigationController = SideMenuNavigationController(rootViewController: menuViewController)

    // Configure the SideMenu
    SideMenuManager.default.menuLeftNavigationController = menuNavigationController
    SideMenuManager.default.menuRightNavigationController = nil // You can also have a right menu
    SideMenuManager.default.menuPresentMode = .menuSlideIn // Set the menu presentation mode
    SideMenuManager.default.menuAnimationType = .default // Set the animation type
    SideMenuManager.default.menuFadeStatusBar = true // Fade the status bar
    SideMenuManager.default.menuWidth = UIScreen.main.bounds.width * 0.8 // Set the menu width
    SideMenuManager.default.menuWidth = UIScreen.main.bounds.width * 0.8 // Set the menu width
    SideMenuManager.default.navigationBar.isHidden = true // Hide the navigation bar in the SideMenu
    SideMenuManager.default.delegate = self // Set the delegate to handle menu events

    // Customize the appearance (optional)
    SideMenuManager.default.menuLeftNavigationController!.navigationBar.barTintColor = .lightGray // Set the background color
    SideMenuManager.default.menuLeftNavigationController!.navigationBar.tintColor = .white // Set the tint color
  }

  // SideMenuNavigationControllerDelegate method
  func sideMenuWillAppear(menu: SideMenuNavigationController, animated: Bool) {
    print("SideMenu will appear")
  }

  func sideMenuDidAppear(menu: SideMenuNavigationController, animated: Bool) {
    print("SideMenu did appear")
  }

  func sideMenuWillDisappear(menu: SideMenuNavigationController, animated: Bool) {
    print("SideMenu will disappear")
  }

  func sideMenuDidDisappear(menu: SideMenuNavigationController, animated: Bool) {
    print("SideMenu did disappear")
  }
}

class MenuViewController: UIViewController, UITableViewDelegate, UITableViewDataSource {

  @IBOutlet weak var tableView: UITableView!

  override func viewDidLoad() {
    super.viewDidLoad()

    // Set up table view
    tableView.delegate = self
    tableView.dataSource = self
    tableView.register(UITableViewCell.self, forCellReuseIdentifier: "cell")
  }

  // UITableViewDataSource methods
  func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
    return 3 // Number of menu items
  }

  func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    let cell = tableView.dequeueReusableCell(withIdentifier: "cell", for: indexPath)
    cell.textLabel?.text = "Item (indexPath.row + 1)"
    return cell
  }

  // UITableViewDelegate method
  func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
    // Handle menu item selection
    // ...
    // Dismiss the SideMenu
    dismiss(animated: true)
  }
}
