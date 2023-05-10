---
layout: post
title:  "SOLID in practice"
date:   2023-05-10 06:00:00 +0300
categories: []
tags: [CleanCode]
---

Example of code breaking SOLID, refactor to fix each.
- FriendListVC is breaking SRP because it will change when the UI changes, but also when the API contract changes
- it deals with different levels of abstraction: network requests, parsing, presentation logic, UI
- FriendsListVC is breaking OCP because we cannot add cache / local loading without changing its code
- LSP ?
- ISP NetworkManageable some clients might need only GET or only POST
- DIP FriendListViewController depends directly on NetworkManager, we can't replace with a different type or a test double

protocol NetworkManageable {
	func get(url: URL, completion: @escaping (Result<(Data, HTTPURLResponse), Error>) -> Void)
	func post(url: URL, body: [String: Any], completion: @escaping (Result<(Data, HTTPURLResponse), Error>) -> Void)
}


class NetworkManager: NetworkManageable {
	static var shared: NetworkManager

	func get(url: URL, completion: @escaping (Result<(Data, HTTPURLResponse), Error>) -> Void) {
		...
	}

	func func post(url: URL, body: [String: Any], completion: @escaping (Result<(Data, HTTPURLResponse), Error>) -> Void) {
		...
	}
}

class FriendListViewController: UITableViewController {
	override func viewDidLoad {
		super.viewDidLoad()

		NetworkManager.shared.get(url) { result in
			switch result {
				case let .success(data, response):
					guard response.httpStatusCode == 200 else ...
					let friends = try JSONDecoder().decode(data, into: [Friend].self)
					self.friends = friends
					self.tableView.reloadData()
			}
		}
	}
}