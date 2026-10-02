# Community content and trust

Plugins and `.csx` scripts execute local code. They are not sandboxed and can access files and network resources with the permissions of the running application.

Opening an installed plugin's settings can load its module even when its working hooks are disabled. Disabling a plugin is not equivalent to removing untrusted code from the installation.

Mods can write to destinations described in their operations. Review targets outside the game folder, especially `${game_data_path}`, `${user_path}`, custom placeholders, and absolute paths. Hard copy and extraction operations can clear their destination before writing.

Import validation checks format and package paths. It is not malware scanning. A one-click confirmation confirms the requested source; it does not certify the package's author or behavior.

**Analyze Actual Launch Result** uses temporary game copies, but script execution still has local permissions. Only inspect executable content this way when you trust it.

Keep game saves and important personal files backed up independently. Restoration protects tracked mod operations; arbitrary code can perform changes outside that tracking.
