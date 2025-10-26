#!/usr/bin/env python3
import sys
import argparse

def count_stream(stream):
    lines = 0
    words = 0
    bytes_ = 0
    for line in stream:
        lines += 1
        words += len(line.split())
        bytes_ += len(line.encode())
    return lines, words, bytes_

def print_counts(counts, name, args, show_all):
    lines = counts[0]
    words = counts[1]
    bytes_ = counts[2]
    parts = []
    if args.l or show_all:
        parts.append(f"{lines:7}")
    if args.w or show_all:
        parts.append(f"{words:7}")
    if args.c or show_all:
        parts.append(f"{bytes_:7}")
    if name:
        parts.append(name)
    output = "".join(parts)
    print(output)

def main():
    parser = argparse.ArgumentParser(description="Count lines, words, and bytes in files (like Unix wc).")
    parser.add_argument("files", nargs="*", help="input file(s), defaults to stdin")
    parser.add_argument("-l", action="store_true", help="print the newline counts")
    parser.add_argument("-w", action="store_true", help="print the word counts")
    parser.add_argument("-c", action="store_true", help="print the byte counts")
    args = parser.parse_args()

    # if no flags, show everything
    show_all = not (args.l or args.w or args.c)
    total_lines = 0
    total_words = 0
    total_bytes = 0

    # read from stdin if no files
    if not args.files:
        counts = count_stream(sys.stdin)
        total_lines += counts[0]
        total_words += counts[1]
        total_bytes += counts[2]
        print_counts(counts, None, args, show_all)
        return

    # go through each file
    for name in args.files:
        try:
            f = open(name, "r", encoding="utf-8", errors="replace")
            counts = count_stream(f)
            f.close()
            total_lines += counts[0]
            total_words += counts[1]
            total_bytes += counts[2]
            print_counts(counts, name, args, show_all)
        except OSError as e:
            print(f"wc: {name}: {e.strerror}", file=sys.stderr)

    # print total if multiple files
    if len(args.files) > 1:
        totals = [total_lines, total_words, total_bytes]
        print_counts(totals, "total", args, show_all)

if __name__ == "__main__":
    main()
