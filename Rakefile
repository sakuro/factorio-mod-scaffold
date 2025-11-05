# frozen_string_literal: true

require "json"
require "rake/clean"

mod_license = "<%= $mod_license %>"
mod_category = "<%= $mod_category %>"
mod_tags = %w[<%= $mod_tags.split(?,).join(' ') %>]

info = JSON.parse(File.read("info.json"))
mod_name = info["name"]
mod_version = info["version"]
dist_dir = "dist"
archive = "#{dist_dir}/#{mod_name}_#{mod_version}.zip"

CLOBBER.include(dist_dir)

archive_sources = %x[git archive --format=tar HEAD | tar -t -f -].lines(chomp: true)

directory dist_dir

file archive => [dist_dir, *archive_sources] do |t|
  prefix = File.basename(t.name, ".zip")
  sh "git archive --prefix #{prefix}/ HEAD -o #{t.name}"
end

desc "Build MOD archive"
task build: archive

desc "Install MOD locally"
task install: archive do |t|
  paths = JSON.parse(%x[bin/factorix path --json])
  cp t.prerequisites.first, paths["mod_dir"]
  sh "bin/factorix", "mod", "enable", mod_name
end

namespace :release do
  desc "Publish MOD to Factorio MOD Portal"
  task portal: :build do
    abort "release:portal task must be run from GitHub Actions" unless ENV["GITHUB_ACTIONS"]

    source_url = %x[git remote get-url origin].chomp

    sh("bin/factorix", "mod", "upload", archive,
      "--category", mod_category,
      "--license", mod_license,
      "--source-url", source_url,
      "--description", File.read("README.md"))

    sh("bin/factorix", "mod", "edit", mod_name,
      "--summary", info["description"],
      "--tags", mod_tags.join(","))
  end

  desc "Create GitHub Release"
  task github: :build do
    abort "release:github task must be run from GitHub Actions" unless ENV["GITHUB_ACTIONS"]

    tag = "v#{mod_version}"
    seen_section = false
    notes = IO.popen(%W[bundle exec factorix mod changelog extract --version #{mod_version}], &:read)
      .lines(chomp: true)
      .filter_map { |line|
        case line
        when /^Version:/, /^  Date:/ then nil
        when /^  [A-Za-z].*:$/
          prefix = seen_section ? "\n" : ""
          seen_section = true
          "#{prefix}### #{line.strip.delete_suffix(":")}"
        when /^    - /
          line.sub(/^    /, "")
        end
      }
      .join("\n")

    sh("gh", "release", "create", tag,
      "--title", "#{mod_name} #{tag}",
      "--notes", notes,
      archive)
  end
end
