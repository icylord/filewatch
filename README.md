# Filewatch <a href="#"><img src="https://img.shields.io/badge/C++-11-blue.svg?style=flat-square"></a>

| Branch        | Status        |
| ------------- |:-------------:|
| **Master**    | [![Master](https://travis-ci.org/ThomasMonkman/filewatch.svg?branch=master)](https://travis-ci.org/ThomasMonkman/filewatch) |
| **Develop**   | [![Develop](https://travis-ci.org/ThomasMonkman/filewatch.svg?branch=develop)](https://travis-ci.org/ThomasMonkman/filewatch) |

Single header folder/file watcher in C++11 for windows and linux
#### Install:
Drop [FileWatch.hpp](https://github.com/ThomasMonkman/filewatch/blob/master/FileWatch.hpp) in to your include path, and you should be good to go.
#### Examples:
- [Simple](#1)
- [Change Type](#2)
- [Using std::filesystem](#3)
- [Works with relative paths](#4)
- [Single file watch](#5)

On linux or none unicode windows change std::wstring for std::string or std::filesystem (boost should work as well).

###### Simple: <a id="1"></a>
```cpp
filewatch::FileWatch<std::wstring> watch(
	L"C:/Users/User/Desktop/Watch/Test"s, 
	[](const std::wstring& path, const filewatch::Event change_type) {
		std::wcout << path << L"\n";
	}
);
```

###### Change Type: <a id="2"></a>
```cpp
filewatch::FileWatch<std::wstring> watch(
	L"C:/Users/User/Desktop/Watch/Test"s, 
	[](const std::wstring& path, const filewatch::Event change_type) {
		std::wcout << path << L" : ";
		switch (change_type)
		{
		case filewatch::Event::added:
			std::cout << "The file was added to the directory." << '\n';
			break;
		case filewatch::Event::removed:
			std::cout << "The file was removed from the directory." << '\n';
			break;
		case filewatch::Event::modified:
			std::cout << "The file was modified. This can be a change in the time stamp or attributes." << '\n';
			break;
		case filewatch::Event::renamed_old:
			std::cout << "The file was renamed and this is the old name." << '\n';
			break;
		case filewatch::ChangeType::renamed_new:
			std::cout << "The file was renamed and this is the new name." << '\n';
			break;
		};
	}
);
```

###### Using std::filesystem: <a id="3"></a>
```cpp
filewatch::FileWatch<std::filesystem::path> watch(
	L"C:/Users/User/Desktop/Watch/Test"s, 
	[](const std::filesystem::path& path, const filewatch::Event change_type) {
		std::wcout << std::filesystem::absolute(path) << L"\n";		
	}
);
```

###### Works with relative paths: <a id="4"></a>
```cpp
filewatch::FileWatch<std::filesystem::path> watch(
	L"./"s, 
	[](const std::filesystem::path& path, const filewatch::Event change_type) {
		std::wcout << std::filesystem::absolute(path) << L"\n";		
	}
);
```

###### Single file watch: <a id="5"></a>
```cpp
filewatch::FileWatch<std::wstring> watch(
	L"./test.txt"s, 
	[](const std::wstring& path, const filewatch::Event change_type) {
		std::wcout << path << L"\n";		
	}
);
```
