{ config, pkgs, ... }:

{
  imports = [ ./hardware-configuration.nix ];

  # BOOT
  boot.loader.systemd-boot.enable = true;
  boot.loader.efi.canTouchEfiVariables = true;

  # NETWORK
  networking.hostName = "nixos";
  networking.networkmanager.enable = true;

  # TIME + LOCALE
  time.timeZone = "America/Bogota";

  i18n = {
    defaultLocale = "es_CO.UTF-8";
  };

  # USER
  users.users.mj = {
    isNormalUser = true;
    description = "mj";
    extraGroups = [ "wheel" "networkmanager" "video" "audio" ];
  };

  # KDE
  services.displayManager.sddm.enable = true;
  services.desktopManager.plasma6.enable = true;

  nixpkgs.config.allowUnfree = true;

  # PRINTING
  services.printing.enable = true;
  services.printing.drivers = with pkgs; [
    epson-escpr
    epson-escpr2
  ];

  # SCANNER
  hardware.sane = {
    enable = true;
    extraBackends = with pkgs; [ sane-airscan ];
  };

  # FLATPAK
  services.flatpak.enable = true;

  # PORTAL
  xdg.portal.enable = true;

  # PACKAGES

  environment.systemPackages = with pkgs; [
    kitty
    fastfetch
    cmatrix
    unzip
    cava
    uwufetch
    tty-clock
    btop

# Servicios de wineport
    mesa
    vulkan-tools
    vulkan-loader
    vulkan-validation-layers

    wineWowPackages.stable
    winetricks
    lutris
  ];

  # AMD + graficos
  hardware.graphics = {
    enable = true;
    enable32Bit = true;
  };

  programs.gamemode.enable = true;

  services.xserver.videoDrivers = [ "amdgpu" ];

  # Audio
  services.pipewire = {
    enable = true;
    pulse.enable = true;
    alsa.enable = true;
    alsa.support32Bit = true;
    jack.enable = true;
  };

  hardware.pulseaudio.enable = false;

  # FIX FONT (esto s   rompe builds)
  fonts.packages = with pkgs; [
    nerd-fonts.fira-code
  ];

  # APPIMAGE
  programs.appimage = {
    enable = true;
    binfmt = true;
  };

  system.stateVersion = "24.11";
}
